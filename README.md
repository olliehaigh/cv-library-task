# CV Library Task
## by Ollie Haigh
Please see above for the files used to create the CV Library task.

To access the files please click the 'Clone or download' button and then 'Download Zip'.

![screenshot](images/readme-img.png)

The files contain all of the HTML, CSS and JavaScript used on this task. There was no use of CSS or JS frameworks and the JS is Vanilla.

For the API portion of the task, there was a lot of trial and error to link up the autocomplete feature and the data for the API. With a lot of googling, videos, reading and trying different techniques, I managed to achieve the end goal. Here is the snippet of JavaScript used to complete the API challenge:

```javascript
function autocomplete(inp
) {

  var currentFocus;
  inp.addEventListener("input", function (e) {
    var a, b, i, val = this.value;
    closeAllLists();
    if (!val) {
      return false;
    }
    currentFocus = -1;
    a = document.createElement("DIV");
    a.setAttribute("id", this.id + "autocomplete-list");
    a.setAttribute("class", "autocomplete-items");
    this.parentNode.appendChild(a);

    const url = "https://api.cv-library.co.uk/v1/locations?q=" + val;
    const response = fetch(url)
      .then((response) => response.json())
      .then(function (locations) {
        return locations;
      })
      .catch(function (error) {
        console.log(JSON.stringify((error)))
      }
      );
    response.then(arr => {
      for (i = 0; i < arr.length; i++) {
        const label = arr[i].label;
        b = document.createElement("DIV");
        b.innerHTML = "<strong>" + label.substr(0, val.length) + "</strong>";
        b.innerHTML += label.substr(val.length);
        b.innerHTML += "<input type='hidden' value='" + label + "'>";
        b.addEventListener("click", function (e) {
          inp.value = this.getElementsByTagName("input")[0].value;
          closeAllLists();
        });
        a.appendChild(b);
      }
    }
    )

  });
  inp.addEventListener("keydown", function (e) {
    var x = document.getElementById(this.id + "autocomplete-list");
    if (x) x = x.getElementsByTagName("div");
    if (e.keyCode === 40) {
      currentFocus++;
      addActive(x);
    } else if (e.keyCode === 38) {
      currentFocus--;
      addActive(x);
    } else if (e.keyCode === 13) {
      e.preventDefault();
      if (currentFocus > -1) {
        if (x) x[currentFocus].click();
      }
    }
  });

  function addActive(x) {
    if (!x) return false;
    removeActive(x);
    if (currentFocus >= x.length) currentFocus = 0;
    if (currentFocus < 0) currentFocus = (x.length - 1);
    x[currentFocus].classList.add("autocomplete-active");
  }

  function removeActive(x) {
    for (var i = 0; i < x.length; i++) {
      x[i].classList.remove("autocomplete-active");
    }
  }

  function closeAllLists(elmnt) {
    var x = document.getElementsByClassName("autocomplete-items");
    for (var i = 0; i < x.length; i++) {
      if (elmnt !== x[i] && elmnt !== inp) {
        x[i].parentNode.removeChild(x[i]);
      }
    }
  }

  document.addEventListener("click", function (e) {
    closeAllLists(e.target);
  });
}
```

I hope you like what I have achieved and look forward to your feedback!