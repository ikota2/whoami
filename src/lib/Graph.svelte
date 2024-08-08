<script>
	import { onMount } from 'svelte';
	import * as d3 from 'd3';
	import svelteLogo from '../assets/svelte.svg';
	import reactRouter from '../assets/react-router.svg';
	import jsLogo from '../assets/js.svg';
	import tsLogo from '../assets/ts.svg';
	import reactLogo from '../assets/react.svg';
	import cssLogo from '../assets/css.svg';
	import sassLogo from '../assets/sass.svg';
	import nextLogo from '../assets/next.svg';
	import htmlLogo from '../assets/html.svg';
	import rnLogo from '../assets/rn.svg';
	import nodejsLogo from '../assets/nodejs.svg';
	import expressLogo from '../assets/express.svg';
	import webstormLogo from '../assets/webstorm.svg';
	import githubLogo from '../assets/github.svg';
	import gitLogo from '../assets/git.svg';
	import postmanLogo from '../assets/postman.svg';
	import threeLogo from '../assets/three.js.svg';
	import toolsLogo from '../assets/tools.svg';
	import codeLogo from '../assets/code.svg';

	let svg;
	const width = 1700;
	const height = 800;


    const nodes = [
        {id: 'tools', group: 'center', name: 'Tools', icon: toolsLogo},
        {id: 'webdev', group: 'center', name: 'WebDev', icon: codeLogo},

        {id: 1, group: 'tools', name: 'WebStorm', icon: webstormLogo},
        {id: 2, group: 'tools', name: 'GitHub', icon: githubLogo},
        {id: 3, group: 'tools', name: 'Postman', icon: postmanLogo},
        {id: 18, group: 'tools', name: 'Git', icon: gitLogo},

        {id: 4, group: 'webdev', name: 'HTML', icon: htmlLogo},
        {id: 5, group: 'webdev', name: 'CSS', icon: cssLogo},
        {id: 6, group: 'webdev', name: 'JavaScript', icon: jsLogo},

        // {id: 7, group: 'css', name: 'CSS Modules', icon: cssLogo},
        {id: 8, group: 'css', name: 'SCSS', icon: sassLogo},

        {id: 9, group: 'js', name: 'TypeScript', icon: tsLogo},
        {id: 10, group: 'js', name: 'Node.js', icon: nodejsLogo},
        {id: 11, group: 'js', name: 'React', icon: reactLogo},
        {id: 12, group: 'js', name: 'Svelte', icon: svelteLogo},
        {id: 20, group: 'js', name: 'Three.js', icon: threeLogo},

        {id: 13, group: 'nodejs', name: 'Express', icon: expressLogo},
        // {id: 14, group: 'nodejs', name: 'NestJS', icon: svelteLogo},

        // {id: 15, group: 'react', name: 'MobX', icon: svelteLogo},
        {id: 16, group: 'react', name: 'React Router', icon: reactRouter},
        {id: 17, group: 'react', name: 'Next.js', icon: nextLogo},
        {id: 19, group: 'react', name: 'React Native', icon: rnLogo}
    ];

    const links = [
        // Tools
        {source: 'tools', target: 1},
        {source: 'tools', target: 2},
        {source: 'tools', target: 3},
        {source: 'tools', target: 18},

        // WebDev
        {source: 'webdev', target: 4},
        {source: 'webdev', target: 5},
        {source: 'webdev', target: 6},

        // CSS
        // {source: 5, target: 7},
        {source: 5, target: 8},

        // JavaScript
        {source: 6, target: 9},
        {source: 6, target: 10},
        {source: 6, target: 11},
        {source: 6, target: 12},
        {source: 6, target: 20},

        // Node.js
        {source: 10, target: 13},
        // {source: 10, target: 14},

        // React
        // {source: 11, target: 15},
        {source: 11, target: 16},
        {source: 11, target: 17},
        {source: 11, target: 19},
    ];


	onMount(() => {
		const simulation = d3.forceSimulation(nodes)
            .force("link", d3.forceLink(links).id(d => d.id).distance(100))
            .force("charge", d3.forceManyBody().strength(-550))
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

		// node.append("text")
        //     .attr("dx", 12)
        //     .attr("dy", ".35em")
        //     .text(d => d.name);

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
