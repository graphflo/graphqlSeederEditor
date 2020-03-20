<template>
	<section class="container">
		<multipane>
			<div id="editor" :style="{ width: '50%', maxWidth: '100%' }">
				<codemirror ref="myCm" :value="code" :options="cmOptions" @ready="onCmReady" @focus="onCmFocus" @input="onCmCodeChange"></codemirror>
			</div>
			<multipane-resizer>
				<!-- <div class="tgl-btn" @click="getSeed">Generate</div> -->
			</multipane-resizer>
			<div id="viewer" :style="{ width: '50%', flexGrow: 1 }">
				<codemirror ref="myCm2" :value="jsonData" :options="cmOptions2"></codemirror>
        <!-- <div class="tgl-btn" @click="getSeed">Generate</div> -->
			</div>
		</multipane>
	</section>
</template>

<script>
import { Multipane, MultipaneResizer } from 'vue-multipane';
import { codemirror } from 'vue-codemirror'
import 'codemirror/lib/codemirror.css'
import 'codemirror/theme/base16-light.css'
import 'codemirror/theme/base16-dark.css'
import 'codemirror/theme/yeti.css'
import 'codemirror/theme/seti.css'
import 'codemirror/theme/gruvbox-dark.css'
import 'codemirror/theme/ambiance.css'
import 'codemirror/theme/darcula.css'
import 'codemirror/theme/dracula.css'
import 'codemirror/theme/material.css'
import 'codemirror/theme/material-darker.css'
import 'codemirror/addon/scroll/simplescrollbars.css'
import 'codemirror/addon/scroll/simplescrollbars.js'
import 'codemirror/addon/fold/foldcode.js'
import 'codemirror/addon/fold/foldgutter.js'
import 'codemirror/addon/fold/brace-fold.js'
import 'codemirror/addon/fold/foldgutter.css'

import 'codemirror/mode/javascript/javascript';

import 'codemirror/addon/lint/lint.css';
import 'codemirror/addon/lint/lint';
import 'codemirror/addon/lint/json-lint';

import 'codemirror/addon/hint/show-hint';
import 'codemirror/addon/lint/lint';
import 'codemirror-graphql/hint';
import 'codemirror-graphql/lint';
import 'codemirror-graphql/mode';

// import  generate  from '../../index'
import generate from '../../dist/browser'
let codesample = `
{
    author {
      firstname: firstName
      lastname: lastName
      # email: email
      age: integer(min: 1, max: 10)
      active:bool
      city: city
      country: country
      phone: phone
      orders  {
        create @repeat(min: 1, max: 5){
          orderdate: date(min: null, max: null )
          ordernumber: objectId
          totalamount: floating(min: 500, max: 1200, fixed: 2, format: "0,0.00")
          orderitems {
            create @repeat(min: 1, max: 7) {
              unitprice: floating(min: 5, max: 200, fixed: 2, format: "0,0.00")
              quantity: integer(min: 1, max: 100)
              product {
                connect: randomID
              }
            }
          }
        }
      }
    }
  }
`;
export default {
	data() {
		return {
			code: codesample,
			cmOptions: {
				tabSize: 4,
				mode: 'graphql',
				theme: 'material',
				lineNumbers: true,
				line: true,
				scrollbarStyle: "simple",
				foldGutter: true,
				gutters: ["CodeMirror-linenumbers", "CodeMirror-foldgutter"]
				// viewportMargin: Infinity
			},
			cmOptions2: {
				tabSize: 4,
				mode: 'application/json',
				theme: 'material',
				lineNumbers: true,
				line: true,
				foldGutter: true,
				scrollbarStyle: "simple",
				gutters: ["CodeMirror-linenumbers", "CodeMirror-foldgutter"]
				// viewportMargin: Infinity
			},
			jsonData: ''
		}
	},
	components: {
				Multipane,
		MultipaneResizer,
		codemirror
	},
	methods: {
		onCmReady(cm) {
			console.log('the editor is readied!', cm);
		},
		onCmFocus(cm) {
			console.log('the editor is focus!', cm);
		},
		onCmCodeChange(newCode) {
			console.log('this is new code', newCode);
			this.code = newCode
		},
		getSeed() {
			// console.log( this.code );
			let result = generate(this.code);
			// console.log( JSON.stringify(result));

			this.jsonData = JSON.stringify(result, null, 2)

		}
	},
	computed: {
		codemirror() {
			return this.$refs.myCm.codemirror
		}
	},
	mounted() {
    this.$bus.$on('test-event', () => {

      this.getSeed()

    })


		console.log('this is current codemirror object', this.codemirror);

		// console.log( this.codemirror.getMode() );/**/

		/*this.$nextTick(() => {
				this.codemirror.refresh();
		});*/


		function setHeight() {
			/*let windowHeight = $(window).innerHeight();*/
			// $('#canvas').css('height', windowHeight - 51);
			/*$('.drop').css('height', windowHeight - 51);

			$('.component-content').css('height', windowHeight - 87);*/
			let x = document.getElementsByClassName("CodeMirror");

			let windowHeight = window.innerHeight;

			console.log("||||||||||||||||||||||||||||||||||||||||||");
			console.log(windowHeight);

			let i;
			for (i = 0; i < x.length; i++) {
				console.log(x);
				x[i].style.height = (windowHeight - 100) + "px";
			}
			// document.getElementsByClassName("CodeMirror").style.height = "50px";
		}

		setHeight();
		/*$(window).resize(function () {
				setHeight();
		});*/
		window.addEventListener('resize', function () {
			setHeight();
		});

		// this.codemirror.setSize("100%", "1000px");
		// you can use this.codemirror to do something...
	}
}
</script>

<style>
.container {
	/*min-height: 100vh;*/
	display: flex;
	/*justify-content: center;
  align-items: center;*/
	/*text-align: center;*/
}

.CodeMirror {
	/* border: 1px solid #000; */
	height: auto;
  font-size: 18px;
}
.CodeMirror-scroll {
	overflow-y: hidden;
	overflow-x: auto;
}
</style>

<style lang="scss" scoped>
.multipane.foo.layout-v .multipane-resizer {
	margin: 0;
	left: 0; /* reset default styling */
	width: 15px;
	background: blue;
}

.multipane.foo.layout-h .multipane-resizer {
	margin: 0;
	top: 0; /* reset default styling */
	height: 15px;
	background: blue;
}

#viewer {
	border: 1px solid #000;
	background-color: #fff;
	.jv-container {
		width: 100%;
	}
}

#editor {
	border: 1px solid #000;
	background-color: #fff;
	height: auto;
}

.multipane {
	width: 100%;
}

.tgl-btn {
	padding: 2px;
	perspective: 100px;
	display: inline-block;
	width: 60px;
	text-align: center;
	line-height: 1.75em;
	position: absolute;
	top: -1px;
	right: 0;
	font-size: 14px;
	cursor: pointer;
	border-radius: 4px;
  z-index: 10000;
}
</style>
