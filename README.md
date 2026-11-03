# FairCost Calculator

An open-source pricing engine that calculates the minimum price a garment factory needs to charge in order to pay its workers a living wage, and produces a quote that shows the math.

> **Status: concept. Looking for contributors.**

## Why this exists

Many brands set a target price for a garment and push suppliers to meet it. Labor is usually the cost that gives way. When the price is too low to cover wages, the difference comes out of workers' pay, their overtime, or their safety.

Suppliers often don't have the costing tools to show, line by line, what a garment costs to make when workers earn a living wage. Without those numbers, a price negotiation is one side's figure against the other's. FairCost aims to give suppliers and unions a transparent, reproducible calculation, and a written record when a brand insists on a price below it.

## Who it's for

- Suppliers who want to price orders responsibly and be able to defend that price
- Trade unions and worker representatives in wage and pricing discussions
- Researchers and organizations working on responsible purchasing practices, such as the Better Buying Institute

This project is independent and is not affiliated with or endorsed by any of the organizations named here.

## Core features

**Living wage benchmarks.** FairCost looks up the living wage benchmark for the production region, for example from the Global Living Wage Coalition's Anker-methodology benchmarks. Every figure is shown with its source and year.

**Labor minute costing.** The supplier enters the garment type and complexity, or a known Standard Allowed Minute (SAM) value, along with their line efficiency. FairCost calculates the direct labor cost per garment that is needed to pay the benchmark wage.

**Exportable quotes.** FairCost generates a PDF quote that breaks down the Cut, Make, Trim (CMT) cost into labor, overhead, and margin. Each quote records its inputs, the benchmark it used, a timestamp, and a hash, so anyone can reproduce the calculation later. If a brand rejects the quote, the supplier and union have a dated record showing that the requested price was below what a living wage requires.

## How the calculation works

```
wage cost per minute     = monthly wage cost per worker / working minutes per month
labor minutes per garment = SAM / line efficiency
labor cost per garment    = labor minutes per garment × wage cost per minute
CMT price                 = labor cost + overhead + margin
```

The monthly wage cost per worker is the living wage benchmark, grossed up for taxes and mandatory deductions, plus employer social contributions.

### Example

These numbers are made up for illustration. LCU means local currency units.

| Input | Value |
| --- | --- |
| Monthly wage cost per worker | 48,000 LCU (a 40,000 LCU benchmark plus 20% for deductions and employer contributions) |
| Working time | 26 days × 8 hours = 12,480 minutes per month |
| SAM | 20 minutes |
| Line efficiency | 60% |

- Wage cost per minute: 48,000 / 12,480 ≈ **3.85 LCU**
- Labor minutes per garment: 20 / 0.60 ≈ **33.3 minutes**
- Labor cost per garment: 33.3 × 3.85 ≈ **128 LCU**, before overhead and margin

A real costing also has to account for paid leave, public holidays, overtime premiums, and indirect labor such as supervisors and quality control. These are on the [roadmap](#roadmap).

## Data sources to evaluate

- Global Living Wage Coalition benchmark reports (Anker methodology)
- WageIndicator Foundation living wage data
- Asia Floor Wage Alliance figures
- **SAM values:** most industry SAM databases are proprietary. We will need an open reference set of typical SAM ranges by garment type, contributed and reviewed by industrial engineers.

Most living wage benchmarks are published as reports rather than through APIs. The first version will probably ship a curated dataset with full citations, plus adapters for any APIs that exist. We will check each source's license before redistributing its data.

## Related work

FairCost should build on existing methods rather than invent new ones. Examples include the Fair Wear Foundation's labour minute costing approach and the commitment by ACT (Action, Collaboration, Transformation) member brands to ring-fence labor costs in their pricing.

## Proposed architecture

```
Web app (Next.js or Nuxt)
  → serverless API
      → calculation engine (standalone, tested library)
      → versioned living wage dataset (with citations)
  → PDF quote generation (Puppeteer or jsPDF)
```

The calculation engine will be a separate library, so that other tools and organizations can use it in their own systems.

## Roadmap

1. Build the calculation engine as a library, with tests that include the worked example above.
2. Curate a living wage dataset for a few production countries, with citations.
3. Build a web interface for entering garment details and viewing the cost breakdown.
4. Add PDF quote export with reproducible inputs.
5. Account for paid leave, holidays, indirect labor, and overtime premiums, and support multiple currencies.
6. Have industrial engineers, unions, and wage experts review the methodology.

## Contributing

Help is especially welcome from:

- Industrial engineers and garment costing specialists
- TypeScript and web developers
- Living wage researchers
- Translators, so suppliers and unions can use the tool in their own languages

Please open an issue to discuss larger changes before starting work on them.

## Disclaimer

FairCost produces estimates from the inputs and benchmarks you give it. It is not legal or financial advice. A living wage benchmark is usually higher than the legal minimum wage, and is not itself a legal requirement in most countries.

## License

[MIT](LICENSE)
