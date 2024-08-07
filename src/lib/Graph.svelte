<script>
	import { onMount } from 'svelte';
	import * as d3 from 'd3';
	import svelteLogo from '../assets/svelte.svg';
	import wineLogo from '../assets/wine.svg';

	let svg;
	const width = 800;
	const height = 600;

	const nodes = [
		{id: 'tools', group: 'center', name: 'Tools', icon: wineLogo},
		{id: 'frameworks', group: 'center', name: 'Frameworks', icon: wineLogo},
		{id: 1, group: 'tools', name: 'WebStorm', icon: svelteLogo},
		{id: 2, group: 'tools', name: 'GitHub', icon: svelteLogo},
		{id: 3, group: 'tools', name: 'Postman', icon: svelteLogo},
		{id: 4, group: 'frameworks', name: 'React', icon: svelteLogo},
		{id: 5, group: 'frameworks', name: 'Svelte', icon: svelteLogo},
		{id: 6, group: 'frameworks', name: 'CSS Modules', icon: svelteLogo}
	];

	const links = [
		{source: 'tools', target: 1},
		{source: 'tools', target: 2},
		{source: 'tools', target: 3},
		{source: 'frameworks', target: 4},
		{source: 'frameworks', target: 5},
		{source: 'frameworks', target: 6}
	];

	onMount(() => {
		const simulation = d3.forceSimulation(nodes)
            .force("link", d3.forceLink(links).id(d => d.id).distance(150))
            .force("charge", d3.forceManyBody().strength(-300))
            .force("center", d3.forceCenter(width / 2, height / 2))
            .force("x", d3.forceX().strength(0.1))
            .force("y", d3.forceY().strength(0.1));

		const svgElement = d3.select(svg)
            .attr("width", width)
            .attr("height", height);

		const link = svgElement.append("g")
            .attr("class", "links")
            .selectAll("line")
            .data(links)
            .enter().append("line")
            .attr("class", "link")
            .style("stroke", "#999")
            .style("stroke-opacity", 0.6)
            .style("stroke-width", "1px");


		const node = svgElement.append("g")
            .attr("class", "nodes")
            .selectAll("g")
            .data(nodes)
            .enter().append("g")
            .call(d3.drag()
            .on("start", dragstarted)
            .on("drag", dragged)
            .on("end", dragended));

		node.append("image")
            .attr("xlink:href", d => d.icon)
            .attr("width", 40)
            .attr("height", 40)
            .attr("x", -20)
            .attr("y", -20);

		node.append("text")
            .attr("dx", 12)
            .attr("dy", ".35em")
            .text(d => d.name);

		simulation.on("tick", () => {
			link.attr("x1", d => d.source.x)
			.attr("y1", d => d.source.y)
			.attr("x2", d => d.target.x)
			.attr("y2", d => d.target.y);

			node.attr("transform", d => `translate(${d.x},${d.y})`);
		});

		function dragstarted(event, d) {
			if (!event.active) simulation.alphaTarget(0.3).restart();
			d.fx = d.x;
			d.fy = d.y;
		}

		function dragged(event, d) {
			d.fx = event.x;
			d.fy = event.y;
		}

		function dragended(event, d) {
			if (!event.active) simulation.alphaTarget(0);
			d.fx = null;
			d.fy = null;
		}
	});
</script>

<style>
	.node image {
		pointer-events: none;
	}
	text {
		font: 12px sans-serif;
		pointer-events: none;
	}
</style>

<svg bind:this={svg}></svg>
