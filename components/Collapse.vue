<style>
.collapse {
	/* margin-bottom: 2px; */
    border-bottom: 2px solid #3b3e41;;
    /* border-top: 1px solid #3b3e41; */
}
.collapse .collapse-header {
	padding: 10px 10px 10px 40px;
	background: #263238;
	border-radius: 3px;
	position: relative;
    font-size: 16px;
	font-weight: bold;
    color: #42b983;
    cursor: pointer;
}
.collapse .collapse-header > div {
	display: flex;
	justify-content: space-between;
	align-items: center;
}
.collapse .collapse-header h3 {
	font-size: 14px;
	font-weight: bold;
}
.collapse .collapse-header::before {
	-moz-transition: all 0.2s;
	-o-transition: all 0.2s;
	-webkit-transition: all 0.2s;
	transition: all 0.2s;
	content: url("../assets/arrow-down.svg");
	position: absolute;
	font-size: 0.4em;
	top: calc(50% - 0.4em);
	left: 20px;
	color: #42b983;
	-moz-transform: rotate(-90deg);
	-o-transform: rotate(-90deg);
	-ms-transform: rotate(-90deg);
	-webkit-transform: rotate(-90deg);
	transform: rotate(-90deg);
}
.collapse.is-active .collapse-header::before {
	-moz-transform: rotate(0deg);
	-o-transform: rotate(0deg);
	-ms-transform: rotate(0deg);
	-webkit-transform: rotate(0deg);
	transform: rotate(0deg);
}

.collapse .collapse-content {
    padding: 15px;
    background-color: #263238;
	/* border-left: 2px solid #f7f7f7;
	border-bottom: 2px solid #f7f7f7;
	border-right: 2px solid #f7f7f7;
	border-bottom-left-radius: 3px;
	border-bottom-right-radius: 3px; */
     /* background-color: #34495e;
    color: #c5c9d0; */
}

.collapse .collapse-content-box {
	-moz-transition: all 0.2s;
	-o-transition: all 0.2s;
	-webkit-transition: all 0.2s;
	transition: all 0.2s;
	padding: 20px 20px;
	/* border: 2px solid #42b983; */
	/* border-bottom: 2px solid #f7f7f7;
	border-right: 2px solid #f7f7f7; */
	/* border-bottom-left-radius: 8px; */
	border-radius: 8px;
    background-color: #42b98314;
    color: #ffffff;
}

</style>

<template>
	<div class="collapse collapse-item" :class="{ 'is-active': active }">
		<div class="collapse-header touchable" role="tab" :aria-expanded="active ? 'true' : 'false'" @click.prevent="toggle">
			<slot name="collapse-header"></slot>
		</div>
		<transition name="fade">
			<div class="collapse-content" v-if="active">
				<div class="collapse-content-box">
					<slot name="collapse-body"></slot>
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
export default {
	name: "Collapse",
	data() {
		return {
			active: false
		}
	},
	props: {
		selected: {
			type: Boolean,
			required: true,
			default: false
		}
	},
	created() {
		this._isCollapseItem = true
		this.active = this.selected
	},
	ready() {
		if (this.active) {
			this.$emit('collapse-open', this.index)
		}
	},
	methods: {
		toggle() {
			this.active = !this.active
			if (this.active) {
				this.$emit('collapse-open', this.index)
			}
		}
	}
}
</script>