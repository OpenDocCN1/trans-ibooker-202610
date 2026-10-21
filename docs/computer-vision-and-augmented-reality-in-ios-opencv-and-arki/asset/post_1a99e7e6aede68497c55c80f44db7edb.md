
Here we will find the first non-zero entry:

```
//find blob////////////////////////////////////////////
unsigned int index = 0;
while (index < blob.elements_number) {
    unsigned int N = (unsigned int)blob.elements_number;
    for (unsigned int i = index; i < N; i++) {
        add_up_neighbour(blob, i);
        add_right_neighbour(blob, i);
        add_down_neighbour(blob, i);
        add_left_neighbour(blob, i);
    }
    index = N;
}
remove_blob_from_image(blob);

if (blob.elements_number > min_elements_per_blob) {
    blob.area = (unsigned int)blob.elements_number;
    blob.elements.reserve(blob.elements_number);
    m_blobs.push_back(blob);
}
```

Now we will find the blob in the given code.

An add * neighbour () processes look for the picture component that is identical to an ith Component within the present blob upwards, below, left, or right, then attach this towards a Blob components array:

```
inline unsigned int ImageBlobs::add_up_neighbour(struct Blob& blob, unsigned int i)
{
    const struct Element& element = blob.elements[i];
    if (element.coord.y - 1 < 0)
        return 0;
    else if ((*m_image)(element.coord.y - 1, element.coord.x) > 0) {
        struct Element new_element;
        new_element.coord.x = element.coord.x;
        new_element.coord.y = element.coord.y - 1;
        if (has_neighbour(element, new_element) == false) {
            int index = is_element_present(blob, new_element);
            if (index >= 0) {
                blob.elements[index].neighbs.push_back(element);
                return 0;
            }
            new_element.neighbs.push_back(element);
            blob.elements_number++;
            blob.elements.push_back(new_element);
            return 1;
        }
        else
            return 0;
    }
    else
        return 0;
}
```

Is_element_present() as well as has_neighbour() decide whether the same latest component has already been existing inside the blob:

```
inline int ImageBlobs::is_element_present(const struct Blob& blob,
    const struct Element& new_element) const
{
    //int index = 0;
    for(int i = (int)blob.elements_number - 1; i >= 0; i--) {
        const struct Element& element = blob.elements[i];
        if (element.coord.x == new_element.coord.x &&
            element.coord.y == new_element.coord.y) {
            //wprintf(L" %d\n", blob.elements_number - 1 - i);
            return i;
        }
        //if (++index > 2) //inspect at least 2 last elements
        //    break;
    }
    return -1;
}

inline bool ImageBlobs::has_neighbour(const struct Element& element,
    const struct Element& new_element) const
{
    unsigned int N = (unsigned int)element.neighbs.size();
    if (N > 0) {
        for (unsigned int i = 0; i < N; i++) {
            if (element.neighbs[i].coord.x == new_element.coord.x &&
                element.neighbs[i].coord.y == new_element.coord.y)
                return true;
        }
        return false;
    }
    else
        return false;
}
```

## Conclusions

I’ve chosen a fixed background (Figure 5-2) as well as conducted a few object tracking tests using items in front of me. The background that I utilized is shown in the following. This became averaged across a long amount of duration.

![](img/7d8e54ee4578088e252f4c75b5cd4c88_105_0.png)

### Figure 5-2. Background

At 66.67 frames per second, two items (display cleaners as well as mobile devices) were tracked (Figure 5-3).

![](img/7d8e54ee4578088e252f4c75b5cd4c88_105_1.png)

### Figure 5-3. Object Tracking in Videos

At 71.43 frames per second, a mobile energy connector was tracked (Figure 5-4). Since the cord seems to be so thin, it is not observed by erosion and dilation techniques.

![](img/7d8e54ee4578088e252f4c75b5cd4c88_106_0.png)

Figure 5-4. Object Tracking in Videos

The following is for a few pens as well as locks: 90.91 frames per second (Figure 5-5).

![](img/7d8e54ee4578088e252f4c75b5cd4c88_107_0.png)

Figure 5-5. Tracking Different Objects

Three distinct items were observed at 29.41 frames per second in a quite complicated situation (Figure 5-6).

![](img/7d8e54ee4578088e252f4c75b5cd4c88_108_0.png)

Figure 5-6. Tracking Different Objects

At 55.56 fps, a Jimi Hendrix and a 9v power were tracked (Figure 5-7).

![](img/7d8e54ee4578088e252f4c75b5cd4c88_109_0.png)

Figure 5-7. Tracking Different Objects

Other background configurations are as follows (Figure 5-8):

![](img/7d8e54ee4578088e252f4c75b5cd4c88_109_1.png)

Figure 5-8. Another Complex Background

At 90.91fps, numerous 9v batteries, rolling upon on rug, were observed (Figure 5-9). Due to a limitation on the quantity of components, a battery was kept unobserved.

![](img/7d8e54ee4578088e252f4c75b5cd4c88_110_0.png)

Figure 5-9. Another Complex Background

At 90.91 frames per second, a battery connector, LCD-TFT filter, and 9v battery were observed (Figure 5-10). A minuscule one went unobserved once more, this time because of erosion and dilation techniques.

![](img/7d8e54ee4578088e252f4c75b5cd4c88_111_0.png)

Figure 5-10. Another Complex Background

Then after 83.33 fps, candles as well as cellphones were observed (Figure 5-11).

![](img/7d8e54ee4578088e252f4c75b5cd4c88_112_0.png)

Figure 5-11. Another Complex Background

As you'll observe a larger entity, much longer it takes to measure a blobs' components. It also doesn't deal with shadows by objects upon on white back wall as well as a table. Highly prominent reflections, but on the other hand, would be tracked as being related to the item.

## Points of Concern

One could enhance an algorithm for tracking the location of an object's bounding box. One can insert an area toward the background scenario photo if it would be fixed for a set duration, as a result, an object being integrated into the scenario. (For example, few objects move to a scenario but remain constant.)

## Custom Barcode Generator/Reader

### Utilizing C++, Build and Read Barcodes

We’ll show you how to programmatically create barcodes in C++ within the chapter. You’ll also discover ways to build a barcode reader or scanner in the C++ program (Figure 5-12).

![](img/7d8e54ee4578088e252f4c75b5cd4c88_113_0.png)

Figure 5-12. Custom Barcode Reader

Information is visually represented in the format in which machines can read it using barcodes. Innovation has proven to be beneficial in such a variety of applications, including automatic check systems within shops as well as in markets. A barcode can now be used on most commercial products. The barcode’s increasing success has led to new applications across several fields. Aspose introduced a barcode generator and reader API for different channels a couple of years back, recognizing the importance of barcodes. The purpose of this chapter is to demonstrate how to automatically produce and interpret barcodes using the barcode generator and reader in C++ programs.

## API for Producing and Reading Barcodes in C++

Assume, barcode with C++ is indeed a functionality C++ API that allows users to build and read a wide range of 1D as well as 2D barcodes. It allows you to produce, configure, as well as send greater-quality pictures through barcodes.

The following is a list of barcode symbologies that Aspose.BarCode with C++ will generate:

- Code128
- Code39 Standard
- Code39 Extended
- Code93 Standard
- Code93 Extended
- Code11
- Codabar
- BooklandEAN
- EAN13
- EAN8
- EAN128
- Interleaved2of5
- Standard2of5
- MSI
- Postnet
- Planet
- UPCA
- UPCE
- EAN14(SCC14)
- SSCC18
- ITF14
- BarCode supplement
- Pdf417
- QR
- Datamatrix
- Matrix 2 of 5
- PZN (Pharma Zentral Nummer, Pharmazentralnummer barcode)
- Deutsche Post Identcode
- Aztec
- Australia Post

## Generating a Barcode Using C++

Utilizing Aspose to build a barcode: With only a few code lines, you can create a barcode. The measures to execute this procedure include the following:

- Make an object of a Barcode Generator class.
- Start an object with encode form and text of the barcode.
- Use the Barcode Generator->Save (System::String) process, and build a barcode.

The following code demonstration illustrates how to produce a barcode in C++:

```
const System::String codeText = u"1234567";
const System::String fileName = u"code39_barcode.jpg";
// Create barcode generator
System::SharedPtr<BarcodeGenerator> barcodeGenerator = System::MakeObject<BarcodeGenerator>(EncodeTypes::Code39Standard, codeText);
barcodeGenerator->get_Parameters()->set_Resolution(300);
// Generate barcode and save as image
barcodeGenerator->Save(fileName);
```
