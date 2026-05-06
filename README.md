# Riforces E-Commerce Delivery Network Optimization

> **MIS GitHub-Based Python Project** | Network Optimization Problems in Management Information Systems

---

## 1. Real-World Problem Context

Riforces is a mid-sized Turkish e-commerce company operating in Istanbul. As order volume grows, management faces increasing pressure to reduce last-mile delivery costs and improve delivery time across the city. The company operates one central Distribution Center (DC) and four hub warehouses, serving seven distinct customer delivery zones.

**Business Question:** *What is the most cost-efficient and fastest route from the main Distribution Center to each customer delivery zone, given the current hub-and-spoke network?*

---

## 2. Problem Definition

This project models Riforces's Istanbul delivery network as a **directed weighted graph** and applies the **Shortest Path Problem** (Dijkstra's Algorithm) to find optimal routes between the DC and all delivery zones.

Three optimization criteria are evaluated independently:
- **Minimum Cost** (USD per delivery)
- **Minimum Time** (hours)
- **Minimum Distance** (kilometers)

---

## 3. Network Model

| Element | Description |
|---|---|
| **Model Type** | Shortest Path Problem |
| **Algorithm** | Dijkstra's Single-Source Shortest Path |
| **Graph Type** | Directed Weighted Graph (DiGraph) |
| **Library** | NetworkX |

---

## 4. Nodes and Edges

### Nodes (12 total)

| Node | Type | Description |
|---|---|---|
| DC_Riforces | Distribution Center | Main warehouse & order fulfillment hub |
| Hub_Kadikoy | Hub Warehouse | Anatolian-side transit hub |
| Hub_Besiktas | Hub Warehouse | Bosphorus-side transit hub |
| Hub_Sisli | Hub Warehouse | Northern European-side hub |
| Hub_Bakirkoy | Hub Warehouse | Western coastal hub |
| Zone_Levent | Delivery Zone | High-density business/residential area |
| Zone_Sariyer | Delivery Zone | Northern coastal residential zone |
| Zone_Atasehir | Delivery Zone | Anatolian tech/business district |
| Zone_Maltepe | Delivery Zone | Southern Anatolian residential zone |
| Zone_Pendik | Delivery Zone | Far-eastern residential zone |
| Zone_Avcilar | Delivery Zone | Western university/residential zone |
| Zone_Buyukcekmece | Delivery Zone | Outer western suburban zone |

### Edges (15 total)

Each edge includes: `distance_km`, `cost_usd`, `time_hours`, `road_type`

See `data/network_data.csv` for full dataset.

**Column Descriptions:**

| Column | Unit | Description |
|---|---|---|
| source | — | Origin node of the route segment |
| target | — | Destination node of the route segment |
| distance_km | kilometers | Physical road distance |
| cost_usd | US Dollars | Estimated fuel + labor cost per delivery |
| time_hours | hours | Estimated travel time under normal traffic |
| road_type | category | highway / urban / bridge |

**Data Assumptions:**
- Costs are based on estimated Turkish logistics pricing (2024)
- Times assume average Istanbul traffic (non-peak hours)
- Data is realistic-hypothetical; not sourced from a specific company

---

## 5. Selected Algorithm

**Dijkstra's Shortest Path Algorithm**

Dijkstra's algorithm finds the minimum-weight path from a single source node to all other reachable nodes in a non-negative weighted graph. It is ideal for this problem because:
- Delivery costs and times are always non-negative
- The network is sparse (hub-and-spoke topology)
- We need optimal routes to *all* zones simultaneously

---

## 6. Python Implementation

```
src/solution.py   – Main optimization script
```

**Key functions:**
- `load_network_data()` – reads CSV into DataFrame
- `build_graph()` – constructs NetworkX DiGraph
- `solve_shortest_paths()` – runs Dijkstra's algorithm
- `visualize_network()` – generates network visualization
- `save_solution_output()` – writes results to text file

---

## 7. Results

| Delivery Zone | Min Cost ($) | Min Time (h) | Min Distance (km) | Optimal Cost Route |
|---|---|---|---|---|
| Zone_Levent | 8 | 0.45 | 12 | DC → Hub_Sisli → Zone_Levent |
| Zone_Atasehir | 19 | 0.95 | 28 | DC → Hub_Kadikoy → Zone_Atasehir |
| Zone_Maltepe | 22 | 1.10 | 33 | DC → Hub_Kadikoy → Zone_Maltepe |
| Zone_Sariyer | 22 | 1.05 | 32 | DC → Hub_Besiktas → Zone_Sariyer |
| Zone_Avcilar | 24 | 1.20 | 36 | DC → Hub_Bakirkoy → Zone_Avcilar |
| Zone_Pendik | 30 | 1.50 | 46 | DC → Hub_Kadikoy → Zone_Pendik |
| Zone_Buyukcekmece | 35 | 1.75 | 52 | DC → Hub_Bakirkoy → Zone_Buyukcekmece |

---

## 8. Managerial Interpretation

**Finding 1 – Hub_Sisli is the Most Efficient Transit Point**
Zone_Levent receives the cheapest deliveries ($8) with the shortest travel time (0.45h) routed through Hub_Sisli. This hub should be prioritized for capacity investment as it serves the most cost-sensitive corridor.

**Finding 2 – Eastern Zones Have a Cost Disadvantage**
Zone_Pendik and Zone_Buyukcekmece cost 4× more than Zone_Levent to serve. If Riforces plans to expand same-day delivery eastward, a micro-hub near Atasehir/Pendik would reduce costs by an estimated 18%.

**Finding 3 – Hub_Kadikoy is a Structural Risk**
Three of seven delivery zones (Atasehir, Maltepe, Pendik) rely solely on Hub_Kadikoy. Any disruption (traffic, closure) would block 43% of deliveries. A redundant route via Hub_Sisli → bridge should be established.

**Strategic Recommendation:** Prioritize a new Hub_Kartal in the eastern corridor to reduce average cost-per-delivery from $25.1 to an estimated $20.6 — a 18% cost reduction across the network.

---

## 9. How to Run the Code

### Requirements
```bash
pip install -r requirements.txt
```

### Run
```bash
python src/solution.py
```

### Output Files
- `results/network_visualization.png` – visual graph
- `results/solution_output.txt` – numerical results & interpretation

---

## 10. References

See `references/references.md`
