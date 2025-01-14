<template>
  <div>
    <b-container>
      <div id="info">
        <div id="mydiv">
          <div id="mydivheader" v-b-modal.modal-shelf1>Test Drag and Drop dynamic layout</div>
          <b-modal id="modal-shelf1" size="xl" hide-footer>
            <div>
              <div style="font-weight: bold;">Name Station: Test Station<label style="font-weight: normal;"></label>
              </div>
              <div style="font-weight: bold;">Model: 024J<label style="font-weight: normal;"></label>
              </div>
              <div style="font-weight: bold;">Sub Message: BC98<label style="font-weight: normal;"></label></div>
              <div style="font-weight: bold;">Conversion Character: 2EE20<label style="font-weight: normal;"></label>
              </div>
              <div style="font-weight: bold;">Priority: 1<label style="font-weight: normal;"></label></div>
            </div>
            <br>
            <b-button variant="outline-primary"
              v-on:click="addBoxstation('top' + index, 'modal-shelf-top' + index, 'div-shelf-top' + index)"
              style="margin: 10px;">Add</b-button>
            <b-button variant="outline-warning"
              v-on:click="editBoxstation('top' + index, 'modal-shelf-top' + index, 'div-shelf-top' + index)"
              style="margin: 10px;">Edit</b-button>
            <b-button variant="outline-danger"
              v-on:click="delBoxstation('top' + index, 'modal-shelf-top' + index, 'div-shelf-top' + index)"
              style="margin: 10px;">Delete</b-button>
          </b-modal>
        </div>
        <div id="mydiv2">
        </div>
      </div>
    </b-container>
  </div>
</template>
<script>
export default {
  data() {
    return {
    }
  },
  mounted() {
    dragElement(document.getElementById("mydiv"));

    function dragElement(elmnt) {
      var pos1 = 0, pos2 = 0, pos3 = 0, pos4 = 0;
      if (document.getElementById(elmnt.id + "header")) {
        /* if present, the header is where you move the DIV from:*/
        document.getElementById(elmnt.id + "header").onmousedown = dragMouseDown;
      } else {
        /* otherwise, move the DIV from anywhere inside the DIV:*/
        elmnt.onmousedown = dragMouseDown;
      }

      function dragMouseDown(e) {
        e = e || window.event;
        e.preventDefault();
        // get the mouse cursor position at startup:
        pos3 = e.clientX;
        pos4 = e.clientY;
        document.onmouseup = closeDragElement;
        // call a function whenever the cursor moves:
        document.onmousemove = elementDrag;
      }

      function elementDrag(e) {
        e = e || window.event;
        e.preventDefault();
        // calculate the new cursor position:
        pos1 = pos3 - e.clientX;
        pos2 = pos4 - e.clientY;
        pos3 = e.clientX;
        pos4 = e.clientY;
        // set the element's new position:
        elmnt.style.top = (elmnt.offsetTop - pos2) + "px";
        elmnt.style.left = (elmnt.offsetLeft - pos1) + "px";
        console.log('position1111', pos3)
        console.log('position2222', pos4)
      }

      function closeDragElement() {
        /* stop moving when mouse button is released:*/
        document.onmouseup = null;
        document.onmousemove = null;
      }
    }
    // let ls = this
    // window.onclick = function (e) {
    //   var i, left = e.clientX, top = e.clientY;
    //   i = document.createElement('div');
    //   i.style.left = left + "px";
    //   i.style.top = top + "px";
    //   i.style.position = "absolute";
    //   i.style.border = '2px solid gray'
    //   i.style.height = "70px";
    //   i.style.width = "70px";
    //   i.style.backgroundColor = "lightblue";
    //   i.setAttribute('id', 'div' + e.clientX + '' + e.clientY)
    //   i.addEventListener('mousedown', ls.handleMouseDown());
    //   i.addEventListener('mouseup', ls.handleMouseUp());
    //   i.addEventListener('mouseleave', ls.handleMouseUp()); // In case the mouse leaves the div
    //   // i.addEventListener("click", ls.myScript())
    //   // i.addEventListener("mouseout", ls.myScript());
    //   document.body.appendChild(i);
    // }
  },
  methods: {
    handleMouseUp () {
      clearTimeout(this.pressTimer);
      console.log('show modal')
    },
    handleMouseDown () {
      this.pressTimer = setTimeout(() => {
        console.log('long press detected')
      }, 1000)
    }
  }
}
</script>
<style>
#mydiv {
  position: absolute;
  z-index: 9;
  background-color: #f1f1f1;
  text-align: center;
  border: 1px solid #d3d3d3;
}

#mydivheader {
  padding: 10px;
  cursor: move;
  z-index: 10;
  background-color: #2196F3;
  color: #fff;
}
</style>