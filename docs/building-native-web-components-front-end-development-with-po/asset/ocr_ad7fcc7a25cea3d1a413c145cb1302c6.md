```
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
                <button type="button" id="addBtn">Add</button><button
                type="button" id="closeBtn">Close</button>
            </form>
        </div>
    </div>`;
}
getStyle() {
    return `
    <style>
        .modal {
            display: none;
            position: fixed;
            z-index: 1;
            padding-top: 100px;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgb(0,0,0);
            background-color: rgba(0,0,0,0.4);
        }
    </style>

```