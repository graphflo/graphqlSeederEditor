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
				<div class="helper-content" v-show="showHelper">
					<h2>Avaliable Functions</h2>
					<div class="function-content">
						<collapse :selected="false" v-for="(_function, index) in avaliableFunctions" :key="index">
							<div slot="collapse-header">{{ index }}</div>
							<div slot="collapse-body">
								<div class="function-title">Description:</div>
								<!-- <div class="function-title">{{ _function.type }}  <span>{{ _function.cat }}</span></div> -->
								<div class="function-description">{{ _function.description }}</div>
								<!-- <div class="function-title">Usage:</div> -->

								<div class="function-title">Arguments:</div>
								<table class="function-arguments">
									<thead>
										<tr>
											<th width="100">Param</th>
											<th width="100">Type</th>
											<th>Details</th>
										</tr>
									</thead>
									<tbody>
										<tr v-for="(attr, index1) in _function.attrs" :key="index1">
											<td>
												{{ attr.name }}
												<!-- <small>(optional)</small> -->
											</td>
											<td>
												<span class="type-hint" :class="{ ['type-hint-'+attr.type]:true}" >{{ attr.type }}</span>
											</td>
											<td>{{ attr.description }}</td>
										</tr>
									</tbody>
								</table>
								<!-- <div class="function-title">Returns:</div>
								<span>{{ _function.returnType }}</span>-->
							</div>
						</collapse>
					</div>
				</div>
				<div v-show="!showHelper">
					<codemirror ref="myCm2" :value="jsonData" :options="cmOptions2"></codemirror>
				</div>

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
// import { generate, func } from '/Users/dandre/Graphflo/graphqlSeeder/dist/browser'
import { generate, func } from 'jsonfarmer'
// import VueCollapse from 'vue2-collapse'
import Collapse from '~/components/Collapse.vue'

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
			avaliableFunctions: func,
			showHelper: false,
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
		codemirror,
		Collapse
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

		this.$bus.$on('toggleHelp', () => {
			console.log(this.showHelper);

			this.showHelper = !this.showHelper
		})

		// this.avaliableFunctions = func


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
 .function-content{
	border-top: 4px solid #3b3e41;
}
.function-title {
	font-size: 16px;
	font-weight: bold;
	color: #91be7e;
	margin-bottom: 5px;
}
.function-title.span {
	font-size: 16px;
}
.function-description {
	font-size: 18px;
	margin-bottom: 15px;
	margin-left: 10px;
	color: #c1c1c2;
}
.function-arguments {
	color: #c1c1c2;
	/* font-weight: bold; */
	width: 100%;
	border-collapse: collapse;
	border-spacing: 0;
	/* font-size: 12px; */
	margin: 10px;
}

.function-arguments th {
	padding: 6px;
	line-height: 1.6;
	text-align: left;
	font-weight: bold;
	border-bottom: 2px solid #263238;
}

.function-arguments td {
	border-top: 1px solid #263238;
	padding: 6px;
	line-height: 1.6;
}

.type-hint {
	display: inline-block;
	text-align: center;
	min-width: 50px;
	margin: 1px 5px;
	padding: 0.2em 0.6em 0.3em;
	font-size: 0.85em;
	font-weight: bold;
	line-height: 1;
	color: #c1c1c2;
	white-space: nowrap;
	vertical-align: baseline;
	border-radius: 2px;
	background: #999;
}

.type-hint-number {
	background: #bd3f42;
}
.type-hint-boolean {
	background: #128327;
}
.type-hint-string {
	background: #3a87ad;
}
.type-hint-object {
	background: #999;
}
.type-hint-array {
	background: #f90;
}

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

.helper-content {
	overflow-y: scroll;
	height: 95vh;
	// background: #3b3e41;
	background: #263238;
	// color: #fff;
	// height: 500px;
}

.helper-content h2 {
	color: #c1c1c2;
}
</style>
