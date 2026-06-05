# ePuSta Elements

`epusta-elements.js` provides the classes `ePuStaInline` and `ePuStaGraph` to display access statistics from an [ePuSta-Server](https://github.com/gbv/ePuSta-Server).

## Installation

```bash
npm install epusta_elements.js
```

Include the script in your HTML page:

```html
<script src="epusta-elements.js"></script>
```

### Dependencies

- **`ePuStaGraph`** requires [Chart.js](https://www.chartjs.org/).
- **`ePuStaInline`** has no external dependencies.

---

## Live Demo

👉 **https://gbv.github.io/ePuSta-Elements/example/**

See [`example/README.md`](example/README.md) for setup instructions.

---

## ePuStaInline

Displays the total access count of a document as an inline number inside any HTML element.

### JavaScript

```js
const element = document.getElementById('my-counter');

const inline = new ePuStaInline(
  element,                                   // DOM element to render into
  'https://epusta.example.org',              // ePuSta-Server base URL
  'oai:example.org:12345',                   // identifier of the document
  '2022-01-01',                              // start date (YYYY-MM-DD); invalid value defaults to 2010-01-01
  '2024-12-31',                              // end date   (YYYY-MM-DD); invalid value defaults to today
  'oas:content:counter'                      // tag query to filter counts
);

inline.requestData();
```

### data Attributes

When the script loads it automatically initialises every element that has the attribute `data-epustaelementtype="ePuStaInline"`:

```html
<span
  data-epustaelementtype="ePuStaInline"
  data-epustaproviderurl="https://epusta.example.org"
  data-epustaidentifier="oai:example.org:12345"
  data-epustafrom="2022-01-01"
  data-epustauntil="2024-12-31"
  data-epustatagquery="oas:content:counter"
></span>
```

| Attribute | Required | Description |
|---|---|---|
| `data-epustaelementtype` | yes | Must be `ePuStaInline` |
| `data-epustaproviderurl` | yes | ePuSta-Server base URL |
| `data-epustaidentifier` | yes | Identifier of the document |
| `data-epustafrom` | no | Start date `YYYY-MM-DD`; defaults to `2010-01-01` |
| `data-epustauntil` | no | End date `YYYY-MM-DD`; defaults to today |
| `data-epustatagquery` | no | Tag query to filter counts |

---

## ePuStaGraph

Renders a bar chart of access counts over time using Chart.js. Granularity must be one of `day`, `week`, `month`, or `year`.

### JavaScript — single data series

Pass the tag query as a plain string. The label defaults to `"Zugriffe"`.

```js
const element = document.getElementById('my-chart');

const graph = new ePuStaGraph(
  element,                                   // DOM element to render into
  'https://epusta.example.org',              // ePuSta-Server base URL
  'oai:example.org:12345',                   // identifier of the document
  'auto',                                    // start date or 'auto' (see below)
  '2024-12-31',                              // end date (YYYY-MM-DD); invalid value defaults to today
  'oas:content:counter',                     // tag query (string → single series)
  'month'                                    // granularity: day | week | month | year
);

graph.requestData();
```

### JavaScript — multiple data series

Pass an array of objects with `label`, `tagquery`, and `color` to draw several bars side by side:

```js
const graph = new ePuStaGraph(
  element,
  'https://epusta.example.org',
  'oai:example.org:12345',
  'auto',
  '2024-12-31',
  [
    { label: 'Full text',  tagquery: 'oas:content:counter',          color: '#4e79a7' },
    { label: 'Abstract',   tagquery: 'oas:content:counter_abstract', color: '#f28e2b' }
  ],
  'month'
);

graph.requestData();
```

### data Attributes

When the script loads it automatically initialises every element that has the attribute `data-epustaelementtype="ePuStaGraph"`. The `data-epustatagquery` attribute corresponds to a single-series tag query.

```html
<div
  data-epustaelementtype="ePuStaGraph"
  data-epustaproviderurl="https://epusta.example.org"
  data-epustaidentifier="oai:example.org:12345"
  data-epustafrom="auto"
  data-epustauntil="2024-12-31"
  data-epustatagquery="oas:content:counter"
  data-epustagranularity="month"
  style="height:300px;"
></div>
```

| Attribute | Required | Description |
|---|---|---|
| `data-epustaelementtype` | yes | Must be `ePuStaGraph` |
| `data-epustaproviderurl` | yes | ePuSta-Server base URL |
| `data-epustaidentifier` | yes | Identifier of the document |
| `data-epustafrom` | no | Start date `YYYY-MM-DD` or `auto` (see below); defaults to `auto` |
| `data-epustauntil` | no | End date `YYYY-MM-DD`; defaults to today |
| `data-epustatagquery` | no | Tag query for a single data series |
| `data-epustagranularity` | yes | `day` \| `week` \| `month` \| `year` |

### `from = "auto"` — automatic start date

When `from` is set to `"auto"` (or left empty), the start date is calculated automatically based on the chosen granularity:

| Granularity | Auto start |
|---|---|
| `day` | 30 days before today |
| `week` | 11 weeks before today |
| `month` | 11 months before today |
| `year` | 9 years before today |
