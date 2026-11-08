
```
use rust_bert::pipelines::sequence_classification::Label;
use rust_bert::pipelines::zero_shot_classification::ZeroShotClassificationModel;
use std::fs::File;
use std::io::BufRead;
use std::io::BufReader;

fn create_db() -> sqlite::Connection {
    let db = sqlite::open(":memory:").unwrap();
    db.execute(
        "CREATE TABLE zeroshotcandidates (id INTEGER PRIMARY KEY, label TEXT)",
    )
    .unwrap();
    ["rock", "pop", "hip hop", "country", "latin"].iter().for_each(|&x| {
        db.execute(&format!("INSERT INTO zeroshotcandidates (label) VALUES ('{}')", x))
            .unwrap();
    });
    db
}

pub fn get_all_zeroshotcandidates() -> Vec<String> {
    let db = create_db();
    let mut candidates: Vec<String> = Vec::new();
    db.iterate("SELECT label FROM zeroshotcandidates", |pairs| {
        for &(_column, value) in pairs.iter() {
            candidates.push(value.unwrap().to_string());
        }
        true
    })
    .unwrap();
    candidates
}

pub fn read_lyrics(file: &str) -> Vec<String> {
    let mut lyrics: Vec<String> = Vec::new();
    let file = File::open(file).expect("Unable to open file");
    let reader = BufReader::new(file);
    for line in reader.lines() {
        lyrics.push(line.unwrap());
    }
    lyrics
}

pub fn classify_lyrics(lyrics: Vec<String>) -> Vec<Vec<Label>> {
    let temp_candidates = get_all_zeroshotcandidates();
    let candidate_labels: Vec<&str> = temp_candidates.iter().map(|s| s.as_str()).collect();
    let lyrics: String = lyrics.join(" ");
    let lyrics: &str = lyrics.as_ref();
    let zero_shot_model = ZeroShotClassificationModel::new(Default::default()).unwrap();
    zero_shot_model.predict_multilabel([lyrics], candidate_labels, None, 128)
}
```

To run everything, you see the different commands exposed via `cargo`:

```
cargo run -- candidates
    Finished dev [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/sqlitehf candidates`
1
rock
2
pop
3
hip hop
4
country
5
latin
```

```
cargo run -- lyrics
   Compiling sqlitehf v0.1.0 (/Users/noahgift/src/rust-mlops-template/sqlite-hf)
    Finished dev [unoptimized + debuginfo] target(s) in 0.76s
     Running `target/debug/sqlitehf lyrics`
Lyrics lyrics.txt
Uh-uh-uh-uh, uh-uh
Ella despidió a su amor
```

```
@noahgift > /workspaces/rust-mlops-template/sqlite-hf (main) $ cargo run -- classify
   Compiling sqlitehf v0.1.0 (/workspaces/rust-mlops-template/sqlite-hf)
    Finished dev [unoptimized + debuginfo] target(s) in 8.76s
     Running `target/debug/sqlitehf classify`
Classify lyrics.txt
rock: 0.06948944181203842
pop: 0.27735018730163574
hip hop: 0.034089818596839905
country: 0.7835917472839355
latin: 0.6906086802482605
```

Yet another example of Rust for MLOps is using PyTorch to load pre-trained models and create image predictions as shown in the `main.rs` [file](https://raw.githubusercontent.com/noahgift/rust-mlops-template/main/pytorch-rust-example/src/main.rs). The PyTorch bindings are straightforward to turn into a pre-trained model tool:

```
/*
Hello world Rust pytorch
Download pre-trained model here:
https://github.com/LaurentMazare/tch-rs/releases/download/mw/resnet18.ot
*/

use anyhow::{bail, Result};
use tch::nn::ModuleT;
use tch::vision::{resnet, imagenet};

pub fn main() -> Result<()> {
    let args: Vec<_> = std::env::args().collect();
    let (weights, image) = match args.as_slice() {
        [_, w, i] => (std::path::Path::new(w), i.to_owned()),
        _ => bail!("usage: main resnet18.ot image.jpg"),
    };
    // Load the image file and resize it to the usual imagenet dimension
    // of 224x224.
    let image = imagenet::load_image_and_resize224(image)?;

    // Create the model and load the weights from the file.
    let mut vs = tch::nn::VarStore::new(tch::Device::Cpu);
    let net: Box<dyn ModuleT> = match
        weights.file_name().unwrap().to_str().unwrap() {
        "resnet18.ot" => Box::new(resnet::resnet18(
            &vs.root(), imagenet::CLASS_COUNT)),
        _ => bail!("unknown model, use a weight file named e.g. resnet18.ot"),
    };
    vs.load(weights)?;

    // Apply the forward pass of the model to get the logits.
    let output = net.forward_t(
        &image.unsqueeze(0), /* train= */ false
    ).softmax(-1, tch::Kind::Float); // Convert to probability.

    // Print the top 5 categories for this image.
    for (probability, class) in imagenet::top(&output, 5).iter() {
        println!("{:50} {:5.2}%", class, 100.0 * probability)
    }
    Ok(())
}
```

To run this example, do the following:

Pre-trained model: `cd` into `pytorch-rust-example` and then run: `cargo run -- resnet18.ot Walking_tiger_female.jpg`. You can see the results in [Figure 10-5](#ch10.xhtml#Figure-8-10-pytorch-invocation).

![](assets/8-10-pytorch-invocation.png)

# Using Rust to Build Tools for MLOps

With some solid ideas on using Rust for deep learning, let’s look at the ecosystem around Rust as it relates to MLOPs.

## Building Containerized Rust Command-Line Tools

Another capability of Rust is containerizing command-line tools. Let’s look at a regular containerized command-line tool and a Rust command-line tool with PyTorch.

The repo for the project is [here](https://github.com/noahgift/rust-docker-cli). The `lib.rs` file holds a function that returns the string “Polo” if the string “Marco” passes in.

```
/* A Marco Polo game. */

/* Accepts a string with a name.
If the name is "Marco", returns "Polo".
If the name is "any other value", it returns "Marco".
*/
pub fn marco_polo(name: &str) -> String {
    if name == "Marco" {
        "Polo".to_string()
    } else {
        "Marco".to_string()
    }
}
```

Next, to invoke the command-line tool, the same pattern as most examples works in this chapter where the library contains the logic, and the main maps the functions to subcommands, in this case, the subcommand `Play`:

```
//A command-line tool to play Marco Polo
use clap::Parser;

#[derive(Parser)]
#[clap(version = "1.0", author = "Noah Gift", about = "A Marco Polo game")]
struct Cli {
    #[clap(subcommand)]
    command: Option<Commands>,
}

#[derive(Parser)]
enum Commands {
    #[clap(version = "1.0", author = "Noah Gift")]
    Play {
        #[clap(short, long)]
        name: String,
    },
}

fn main() {
    let args = Cli::parse();
    match args.command {
        Some(Commands::Play { name }) => {
            let result = containerized_marco_polo_cli::marco_polo(&name);
            println!("{}", result);
        }
        None => println!("No subcommand was used"),
    }
}
```

Finally, the `Dockerfile` is [tiny](https://github.com/noahgift/rust-docker-cli/blob/main/Dockerfile) to turn this project into a containerized tool. The first section of the `Dockerfile` builds the project, and then a smaller container image, `debian:buster-slim` allows for a reduced footprint:

```
FROM Rust:latest as builder
ENV APP containerized_marco_polo_cli
WORKDIR /usr/src/$APP
COPY . .
RUN cargo install --path .

FROM debian:buster-slim
RUN apt-get update && rm -rf /var/lib/apt/lists/*
COPY --from=builder /usr/local/cargo/bin/$APP /usr/local/bin/$APP
ENTRYPOINT [ "/usr/local/bin/containerized_marco_polo_cli" ]
```

Building and running the container is straightforward now.

```
docker build -t marco-polo .
docker run --rm -it marco-polo --help
docker run --rm -it marco-polo play --name Marco
Polo
```

With this knowledge in our toolkit, let’s package our previous PyTorch Rust pre-trained model into a container. The complete example project lives [here](https://github.com/noahgift/rust-mlops-template/tree/main/pytorch-rust-docker).

The main [relevant addition](https://github.com/noahgift/rust-mlops-template/blob/main/pytorch-rust-docker/Dockerfile) is the `Dockerfile` which leverages the existing `Cargo` ecosystem to install PyTorch.

```
FROM Rust:latest as builder
ENV APP pytorch-rust-docker
WORKDIR /usr/src/$APP
COPY . .
RUN apt-get update && rm -rf /var/lib/apt/lists/*
RUN cargo install --path .
RUN cargo build -j 6
```
