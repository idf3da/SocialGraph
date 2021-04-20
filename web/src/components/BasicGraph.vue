<template>
	<div id="3d-graph"></div>
</template>

<script>
	import ForceGraph3D from "3d-force-graph";

	export default {
		name: "BasicGraph",
		mounted() {
			const jsonData = require("../../../data/result.json");
			const graph = ForceGraph3D()(document.getElementById("3d-graph"));

			let temp = {
				nodes: [],
				links: [],
			};

			for (let el of jsonData["links"]) {
				if (el.value > 0.2) {
					temp["links"].push(el);
				}
			}

			temp["nodes"] = jsonData["nodes"];

			graph
				.nodeLabel("id")
				.graphData(temp)
				.nodeResolution(8)
				.onNodeClick((node) => {
					const distance = 40;
					const distRatio = 1 + distance / Math.hypot(node.x, node.y, node.z);
					graph.cameraPosition({ x: node.x * distRatio, y: node.y * distRatio, z: node.z * distRatio }, node, 3000);
				});
			// .linkVisibility((l) => (l.value > 0.4 ? true : false));
		},
	};
</script>
