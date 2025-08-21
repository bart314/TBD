  <style>
    .node circle {
      stroke: #333;
      stroke-width: 1.5px;
    }
    .node text {
      font-size: 12px;
      pointer-events: none;
    }
    .link {
      stroke: #aaa;
      stroke-opacity: 0.6;
    }
  </style>

<svg width="900" height="900"></svg>

  <script src="https://d3js.org/d3.v7.min.js"></script>
<script>
const width = 900, height = 900;

const data = {
  nodes: [
    { id: "Time Based Design", url: "index.html", group: "center" },

    { id: "We and They", url: "we-and-they.html", group: "cluster1" },
    { id: "Fact and Fiction", url: "fact-and-fiction.html", group: "cluster2" },
    { id: "Time and Space", url: "time-and-space.html", group: "cluster3" },
    { id: "World and Reality", url: "world-and-reality.html", group: "cluster4" },
    { id: "Noise and Signal", url: "noise-and-signal.html", group: "cluster5" },
    { id: "Self and Other (Embodiment)", url: "self-and-other.html", group: "cluster6" },
    { id: "Presence and Absence", url: "presence-and-absence.html", group: "cluster7" },
  ],
  links: [
    { source: "Time Based Design", target: "We and They" },
    { source: "Time Based Design", target: "Fact and Fiction" },
    { source: "Time Based Design", target: "Time and Space" },
    { source: "Time Based Design", target: "World and Reality" },
    { source: "Time Based Design", target: "Noise and Signal" },
    { source: "Time Based Design", target: "Self and Other (Embodiment)" },
    { source: "Time Based Design", target: "Presence and Absence" },
  ]
};


const svg = d3.select("svg")
  .attr("viewBox", [0, 0, width, height]);

const color = d3.scaleOrdinal(d3.schemeCategory10);

const simulation = d3.forceSimulation(data.nodes)
  .force("link", d3.forceLink(data.links).id(d => d.id).distance(200))
  .force("charge", d3.forceManyBody().strength(-400))
  .force("center", d3.forceCenter(width / 2, height / 2));

const link = svg.append("g")
  .selectAll("line")
  .data(data.links)
  .enter().append("line")
  .attr("class", "link")
  .attr("stroke-width", 2);

const node = svg.append("g")
  .selectAll("g")
  .data(data.nodes)
  .enter().append("g")
  .attr("class", "node")
  .on("click", (event, d) => {
    if (d.url) {
      window.location.href = d.url; // navigate on click
    }
  });

node.append("circle")
  .attr("r", d => d.group === "center" ? 20 : 14)
  .attr("fill", d => color(d.group));

node.append("text")
  .text(d => d.id)
  .attr("x", 20)
  .attr("y", 5);

simulation.on("tick", () => {
  link
    .attr("x1", d => d.source.x)
    .attr("y1", d => d.source.y)
    .attr("x2", d => d.target.x)
    .attr("y2", d => d.target.y);

  node.attr("transform", d => `translate(${d.x},${d.y})`);
});
</script>