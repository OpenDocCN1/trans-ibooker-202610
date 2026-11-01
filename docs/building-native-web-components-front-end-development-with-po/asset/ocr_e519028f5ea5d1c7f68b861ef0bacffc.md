```
handleAdd() {
    const fTitle = this.root.getElementById('ftitle');
    const fDesc = this.root.getElementById('fdesc');
    this.dispatchEvent(new CustomEvent('add-event', {bubbles: true,
    composed:true, detail: {title: fTitle.value, description: fDesc.
    value}}));

    fTitle.value = '';
    fDesc.value = '';
    this.open = false;
}

handleCancel() {
    this.open = false;
}

showModal(state) {
    if(state) {
        this.modal.style.display = "block";
    } else {
        this.modal.style.display = "none"
    }
}

getTemplate() {
    return `
    ${this.getStyle()}
    <div id="myModal" class="modal">
        <div class="modal-content">
            <form id="myForm">
                <label for="ftitle">Title:</label><br>
                <input type="text" id="ftitle" name="ftitle"><br>
                <label for="fdesc">Description:</label><br>
                <textarea id="fdesc" name="fdesc" rows="4" cols="50">
                </textarea><br/>
                <button type="button" id="addBtn">Add</button><button type=
                "button" id="closeBtn">Close</button>
```