# Specification 'Web of Things (WoT) Thing Description'

Each commit here will sync it to the master, which will expose the content to http://w3c.github.io/wot-thing-description/.

For the draft TD specification for TAG review, please access this [URL](https://cdn.staticaly.com/gh/w3c/wot-thing-description/TD-TAG-review/index.html?env=dev).

To make contributions, please provide pull-requests to the appropriate files,
keeping in mind that some files, most notably `index.html` and `testing/report.html`, 
as well as most files under `visualization`, are
autogenerated and should not be modified directly.
See [github help](https://help.github.com/articles/using-pull-requests/) for 
information on how to create a pull request.

## Specification Rendering

Part of the document is automatically rendered using the [STTL.js](https://github.com/vcharpenay/STTL.js/) RDF template engine and Node.js.
_Any change to the document must be performed on the main HTML template [`index.template.html`](index.template.html)_, and not on `index.html`.
To render `index.html`, along with SVG figures, run: 

```sh
npm run render
```

You can also invoke the rendering script directly:
```sh
./render.sh
```

Requirements: Java 8, Node.js 6, [GraphViz](https://graphviz.org/).

The script will first download and install some dependencies (triple store, Node.js dependencies) and then execute the JS script `render.js`.
The latter should always be execute within `render.sh` since it requires some env variables to be set first.

For Windows users, the script should be run in a [Cygwin shell](http://cygwin.com/). Git package from Cygwin distribution had better not be used. Alternative Git client distribution such as [Git for Windows](https://gitforwindows.org/) works better when you encounter an issue building the document using Cygwin.

## Implementation Report

To generate the implementation report,
including a list of normative assertions,
issue the following command:
```sh
npm run assertions
```
A draft implementation report will be generated and output to
[testing/report.html](testing/report.html)
which will use relative links back up to [index.html](index.html).
The input to this process is [index.html](index.html)
(_not_ `index.html.template`) so make sure to execute `npm run render` first.

For this to work, the assertions need to 
be marked up as in the following examples and follow RFC2119 conventions:
```html
<span class="rfc2119-assertion" id="additional-vocabularies">
  A JSON TD MAY contain additional optional vocabularies that are 
  not in the Thing Description core model.
</span>
<span class="rfc2119-assertion" id="additional-vocabularies-prefix">
  Terms from additional optional vocabularies used in a JSON-TD MUST 
  carry a prefix for identification within the key name
  (e.g., <tt>"http:header"</tt>).
</span>
```

The assertions must be marked up as follows:
* Enclose each assertion in a span.
* Mark the span with unique id.
  It is recommended that the section id be followed
  by a short unique name for the specific assertion.
* Mark the span with a 'class' attribute set to `rfc2119-assertion`.
* Include one (and only one) instance of the RFC2119 keywords (MUST, MAY, etc.)
  in capitals.
This markup does not change the rendering; it just clearly indicates
and uniquely names the assertion.

It is strongly recommended to make assertions independent of context.
In particular, avoid using pronouns or relational expressions
referring to previous statements not included in the assertion.
Such references can always be replaced with their
antecendent ("dereferenced") without changing the meaning,
and this is less ambigious anyway.
For example, instead of using "this serialization", use
"a JSON-TD serialization".

Also, assertions should ideally only constrain one item.
Multiple constraints should be stated in separate sentences.

Note that the above rendering process also assigns each
table entry a unique ID and these are also listed in the 
table included in the implementation report.

Other data, e.g., data from test results, test specifications,
and implementation descriptions, are also needed to complete the 
implementation report.  See [testing/README.md](testing/README.md)
for details.

The generation of the implementation report also generates a CSS file
`testing/atrisk.css`
that highlights at-risk items in the generated `index.html`.  The at-risk
items are listed in `testing/inputs/atrisk.csv`.  If at-risk items are
updated, to update the at-risk highlighting the implementation report
needs to be generated first, and then the rendering.
