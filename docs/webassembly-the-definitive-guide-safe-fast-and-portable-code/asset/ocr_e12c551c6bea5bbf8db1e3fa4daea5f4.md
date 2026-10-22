# Chapter 3. WebAssembly Modules

# A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 3rd chapter of the final book.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the author at wasmbook@bosatsu.net.

> A place for everything and everything in its place.
> —17th Century Proverb

An operating system runs a program usually contained in a compiled form¹. Each operating system has its own format that defines where to start, what data is necessary, and what the instructions are for the different advertised bits of functionality actually are. WebAssembly is no different. In this chapter we are going to look at how the behavior is packaged up and how a host will know what to do with it.

It is possible that software engineers can spend their entire careers ignoring how programs are loaded and executed throught this process. Their world starts and stops at `int main(int argc, char **argv)` or `static void main(String []args)` or even `if __name__ == "__main__":`. These are well-known entry points to programs in C, Java, and Python thus this is where programmers assume responsibility for the control flow. Prior to programs being launched and after they exit, however,