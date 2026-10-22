```
class Widget
{
    // ...
    private:
        void updateCollection( /* some arguments needed to update the collection */ );

        std::vector<Blob> blobs_;
        /* Potentially other data members */
};
```

Ok, let us take a deep breath and just calm down. Although this last approach may seem funny, you should realize that we have now left the range of reasonable arguments.34 If we seriously consider to make use of the preprocessor to hack our way into the private section of the Widget class, then all is lost.

# The True Solution: Separate Concerns

“Ok, Mr. Smarty-Pants, then what should I do to test the private member function? You have already discarded all the options.” No, not all the options. We have not yet discussed the one design approach that I have highlighted in “Guideline 2: Design for Change”: separation of concerns. My approach would be to to extract the private member function from the class and make it a separate entity in our code base. My preferred solution in this case is to extract the member function as a free function:

```
void updateCollection( std::vector<Blob>& blobs
                    , /* some arguments needed to update the collection */ );

class Widget
{
    // ...
    private:
        std::vector<Blob> blobs_;
        /* Potentially other data members */
};
```