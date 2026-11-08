
## Invoking Container

The steps to invoke this container involve running the following commands to build and run it:

```
docker build -t pytorch-rust-docker .
docker run -it pytorch-rust-docker
#runs inside of container
cargo run -- resnet18.ot Walking_tiger_female.jpg
```

## GPU PyTorch Workflows

The synergy of performance gains from Rust really shines when a GPU comes into the mix. This [following repository](https://github.com/nogibjj/rust-pytorch-gpu-template) has many repeatable GPU examples thanks to the excellent toolchain from GitHub Codespaces.

As long as the environmental variable `export TORCH_CUDA_VERSION=cu117` exists, `cargo` builds an NVidia Cuda project version. The following MNIST (Modified National Institute of Standards and Technology database) project shows a very brief [training snippet](https://github.com/nogibjj/rust-pytorch-gpu-template/blob/main/pytorch-mnist/src/mnist_conv.rs):

```
// CNN model. This should reach 99.1% accuracy.

use anyhow::Result;
use tch::{nn, nn::ModuleT, nn::OptimizerConfig, Device, Tensor};

#[derive(Debug)]
struct Net {
    conv1: nn::Conv2D,
    conv2: nn::Conv2D,
    fc1: nn::Linear,
    fc2: nn::Linear,
}

impl Net {
    fn new(vs: &nn::Path) -> Net {
        let conv1 = nn::conv2d(vs, 1, 32, 5, Default::default());
        let conv2 = nn::conv2d(vs, 32, 64, 5, Default::default());
        let fc1 = nn::linear(vs, 1024, 1024, Default::default());
        let fc2 = nn::linear(vs, 1024, 10, Default::default());
        Net { conv1, conv2, fc1, fc2 }
    }
}

impl nn::ModuleT for Net {
    fn forward_t(&self, xs: &Tensor, train: bool) -> Tensor {
        xs.view([-1, 1, 28, 28])
            .apply(&self.conv1)
            .max_pool2d_default(2)
            .apply(&self.conv2)
            .max_pool2d_default(2)
            .view([-1, 1024])
            .apply(&self.fc1)
            .relu()
            .dropout(0.5, train)
            .apply(&self.fc2)
    }
}

pub fn run() -> Result<()> {
    let m = tch::vision::mnist::load_dir("data")?;
    let vs = nn::VarStore::new(Device::cuda_if_available());
    let net = Net::new(&vs.root());
    let mut opt = nn::Adam::default().build(&vs, 1e-4)?;
    for epoch in 1..100 {
        for (bimages, blabels) in m.train_iter(256)
            .shuffle()
            .to_device(vs.device()) {
            let loss = net.forward_t(&bimages, true)
                .cross_entropy_for_logits(&blabels);
            opt.backward_step(&loss);
        }
        let test_accuracy = net
            .batch_accuracy_for_logits(
                &m.test_images, &m.test_labels, vs.device(), 1024);
        println!("epoch: {:4} test acc: {:5.2}%",
            epoch, 100. * test_accuracy,);
    }
    Ok(())
}
```

To run, you `cd` into the `pytorch-mnist` directory and run the following code: `cargo run -- conv`. The result shows a lightening fast training of a model as shown in [Figure 10-6](#ch10.xhtml#Figure-8-13-mnist-pytorch). The `nvidia-smi -l 1` command enabled GPU monitoring, verifying that the GPU is doing the heavy lifting.

![](assets/8-13-mnist-pytorch.png)

Yet another effective Rust GPU workflow is to run the latest version of Stable Diffusion:

* Clone this [repo](https://github.com/LaurentMazare/diffusers-rs).
* Follow these [setup instructions](https://github.com/LaurentMazare/diffusers-rs#clip-encoding-weights):

After completing the previous steps, the download of the model weights and then run:

`cargo run --example stable-diffusion --features clap -- --prompt "A very rusty robot holding a fire torch to notebooks"`

Yet again, you see the power of Rust and GPU as the GPU gets completely saturated, as shown in [Figure 10-7](#ch10.xhtml#Figure-8-14-stable-diffusion-gpu).

![](assets/8-14-stable-diffusion-gpu.png)

The result in [Figure 10-8](#ch10.xhtml#Figure-8-15-flame-robot) is a robot picture that can easily integrate into a complex pipeline due to the high-performance nature of Rust.

![](assets/8-15-flame-robot.png)

## Using Tensorflow Rust

You don’t need only to use PyTorch. To run Tensorflow with Rust, look at this example in this [repo](https://github.com/noahgift/rust-mlops-template/tree/main/tf-rust-example).

First, look at the `Cargo.toml` [file](https://github.com/noahgift/rust-mlops-template/blob/main/tf-rust-example/Cargo.toml). Notice that the `tensorflow` crate is all needed to get started:

```
[package]
name = "tf-rust-cli"
version = "0.1.0"
edition = "2021"
[dependencies]
tensorflow = "0.19.1"
```

Next, in the `main.rs` [file](https://github.com/noahgift/rust-mlops-template/blob/main/tf-rust-example/src/main.rs), a simple example involves only a few lines of code. To see more ideas, visit the official crate [documentation page](https://crates.io/crates/tensorflow), including enabling GPU support (something easily accomplished with GitHub Codespaces with GPU):

```
/*Rust Tensorflow Hello World */

extern crate tensorflow;
use tensorflow::Tensor;

fn main() {
    let mut x = Tensor::new(&[1]);
    x[0] = 2i32;
    //print the value of x
    println!("{:?}", x[0]);
    //print the shape of x
    println!("{:?}", x.shape());
    //create a multidimensional tensor
    let mut y = Tensor::new(&[2, 2]);
    y[0] = 1i32;
    y[1] = 2i32;
    y[2] = 3i32;
    y[3] = 4i32;
    //print the value of y
    println!("{:?}", y[0]);
    //print the shape of y
    println!("{:?}", y.shape());
}
```

## Doing KMeans Clustering with Rust

There are also high-performance scientific libraries for machine learning in Rust. An excellent example of a library is [linfa](https://crates.io/crates/linfa). There are many benchmarked examples.

Simple examples show a similar amount of code as machine learning in Python using [scikit-learn](https://scikit-learn.org/stable/). A key and critical point from the library author is the statement, “No need to have a second language for performance reasons,” as in you don’t need C and Python; everything is fast with just Rust.

The following example from their website shows just how simple and clean the code looks:

```
let (train, valid) = linfa_datasets::diabetes()
    .split_with_ratio(0.9);

// train pure LASSO model with 0.1 penalty
let model = ElasticNet::params()
    .penalty(0.1)
    .l1_ratio(1.0)
    .fit(&train)?;

println!("z score: {:?}", model.z_score());

// validate
let y_est = model.predict(&valid);
println!("predicted variance: {}", y_est.r2(&valid)?);
```

There is a blog post of their k-means example [which shows 25 times faster inference](https://github.com/LukeMathWalker/clustering-benchmarks) than scikit-learn. You can see a command-line tool example Noah created in the following [repository](https://github.com/noahgift/rust-mlops-template/tree/main/linfa-kmeans).

## Final Notes on Rust

Let’s discuss a few additional notes on Rust next.

### Ruff Linter

A new rust-based linter called [ruff](https://github.com/charliermarsh/ruff) can significantly decrease the time it takes to lint a codebase. You can see in [Figure 10-9](#ch10.xhtml#Figure-5-6-ruff) that ruff gives speed improvements anywhere from 12-120x regular python.

![](assets/5-6-ruff.png)

According to the author, the added speed in linting makes “instant” possible for millions of lines of code. In a [blog post about the tool](https://notes.crmarsh.com/python-tooling-could-be-much-much-faster), he wisely says, “Ultimately, my goal with Ruff is to get the Python ecosystem to question the status quo. How long should it take to lint a million lines of code? In my opinion: it should be instant. And if your developer tools were instant, what would that unlock?”

### rust-new-project-template

For beginners, a good idea is for Rust projects to follow this pattern:

1. Create a new repo using Rust New Project Template: [`https://github.com/noahgift/rust-new-project-template`](https://github.com/noahgift/rust-new-project-template)
2. Create a new Codespace and use it
