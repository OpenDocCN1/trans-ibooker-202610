```
<div id="B">Topic B</div>
<div id="C">Topic C</div>
<div id="X">Topic X</div>
</div>
```

2. The second HTML fragment includes topics D, E, and F:

```
<!DOCTYPE html>
<!-- /repo/ch09/dom_test_2.html -->
<div id="content">
<div id="D">Topic D</div>
<div id="E">Topic E</div>
<div id="F">Topic F</div>
</div>
```

3. To create DOMDocument instances from each of the two fragments, we can make a static call; that is, loadHTMLFile. Note that this usage is deprecated in PHP 7 and has been removed in PHP 8:

```
$doc1 = DomDocument::loadHTMLFile( 'dom_test_1.html');
$doc2 = DomDocument::loadHTMLFile('dom_test_2.html');
```

4. Then, we can extract Topic X into $topic and import it into the second document as $new. Next, retrieve the target node; that is, content:

```
$topic = $doc1->getElementById('X');
$new = $doc2->importNode($topic);
$new->textContent= $topic->textContent;
$main = $doc2->getElementById('content');
```

5. Here is where PHP 7 and PHP 8 start to differ. In PHP 7, to move the node, the code must be as follows:

```
// /repo/ch09/php7_dom_changes.php
$main->appendChild($new);
$topic->parentNode->removeChild($topic);
```