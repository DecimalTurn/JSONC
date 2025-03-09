---

layout: default
---

### Examples of invalid JSONC

<pre><code class="language-jsonc">
{
    "name": "Jane Doe", 
    "age": 2/*test*/<span class="hljs-error">5</span>
}</code><pre>

<script>
  // Find the element inside the code tag that has a class of hlsj-number and has number 5 and replace the class with hljs-error
  var nb = document.getElementsByTagName("code")[0].querySelector(".hljs-number")
  
  for (let i = 0; i < nb.length; i++) {
    if (nb[i].textContent === "5") {
      nb[i].classList.replace("hljs-number", "hljs-error");
    }
  }
</script>