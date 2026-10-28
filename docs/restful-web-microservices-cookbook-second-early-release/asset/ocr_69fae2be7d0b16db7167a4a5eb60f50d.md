```
```javascript
error();

elm = d.find("cj");
if(elm) {
    elm.style.display="block";
}
}

// HAL --> HTML
function parse(hal) {
    g.hal = hal;

    title();
    setContext();
    if(g.context!=="") {
        selectLinks("app", "toplinks");
        selectLinks("list", "h-links");
        content();
        embedded();
        properties();
    }
    else {
        alert("Unknown Context, can't continue");
    }

    elm = d.find("hal");
    if(elm) {
        elm.style.display="block";
    }
}

// SIREN --> HTML
function parse(msg) {
    var elm;

    g.siren = msg;

    title();
    getContent();
    links();
    entities();
    properties();
    actions();

    elm = d.find("siren");
    if(elm) {
        elm.style.display="block";
    }
}
```
```