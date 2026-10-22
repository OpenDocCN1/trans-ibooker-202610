pages. We then wrap the ArrayBuffer with a Uint32Array which allows us to fetch our memory values as 4-byte integers. The first element of this is shown in the next <span>. We also wrap our ArrayBuffer in Uint8Array and show the first four bytes. After our discussion above, the details shown in Figure 4-2 should not surprise you.

# Example 4-5. The JavaScript code for our example.

```
function showDetails(mem) {
    var buf = mem.buffer;
    var memEl = document.getElementById('mem');
    var pagesEl = document.getElementById('pages');
    var firstIntEl = document.getElementById('firstint');
    var firstBytesEl = document.getElementById('firstbytes');

    memEl.innerText=buf.byteLength;
    pagesEl.innerText=buf.byteLength / 65536;

    var i32 = new Uint32Array(buf);
    var u8 = new Uint8Array(buf);

    firstIntEl.innerText=i32[0];
    firstBytesEl.innerText= "[" + u8[0] + "," + u8[1] + "," + u8[2] + "," + u8[3] + "]";
};

fetchAndInstantiate('memory.wasm').then(function(instance) {
    var mem = instance.exports.memory;

    var button = document.getElementById("expand");
    button.onclick = function() {
        try {
            mem.grow(1);
            showDetails(mem);
        } catch(re) {
            alert("You cannot grow the Memory any more!");
        };
    };
    showDetails(mem);
});
```