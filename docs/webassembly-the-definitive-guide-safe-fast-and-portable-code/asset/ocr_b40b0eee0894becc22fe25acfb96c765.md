```
brian@tweezer ~/g/w/s/ch03> wat2wasm hello.wat -o hellodebug.wasm --debug-names
brian@tweezer ~/g/w/s/ch03> wasm-objdump -x hellodebug.wasm

hellodebug.wasm:           file format wasm 0x1

Section Details:

Type[1]:
  - type[0] (i32, i32) -> i32
Function[1]:
  - func[0] sig=0 <how_old>
Export[1]:
  - func[0] <how_old> -> "how_old"
Code[1]:
  - func[0] size=7 <how_old>
Custom:
  - name: "name"
  - func[0] <how_old>
  - func[0] local[0] <year_now>
  - func[0] local[1] <year_born>
```

Notice that wat2wasm uses the Custom section to preserve the function and local variable details. Other tools may use this section for their own purposes, but this is how debugging information is usually captured. In Figure 3-3 you can see that there are more bytes in the module because of this Custom section.