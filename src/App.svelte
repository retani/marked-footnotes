<script>
	import marked from 'marked'

	let src=`
# Start 
123 [fn]footnote1[\\fn] 456
  
789 [fn][footnote2](https://example.com)[\\fn]
$ latex $
**Hallo**`

console.log(src)

// \[fn\](.*)\[\\fn\]
// \$+([^\$\n]+?)\$+

	let options = {
		//breaks: true,
		tokenizer: {
			del(src) {
				const match = src.match(/(\[fn\])(.*)(\[\\fn\])/);
				console.log(src, match)
				if (match) {
					return {
						type: 'del',
						raw:  match[0],
						text: match[2]
					};
				}

				// return false to use original codespan tokenizer
				//return false;
			}
		},
		renderer: {
			del(text) {

				return `<sup>${text}</sup>`;
			}
		}
	}

	marked.use(options)
</script>

<main>
	<pre>
	{src}
	</pre>
	<hr>

	{@html marked(src /*,options*/)}
</main>

<style>

</style>