# Univer CLI chart-heavy scrolling repro

This repository contains a synthetic `.univer` workbook that reproduces noticeable vertical scrolling stalls in the local Univer Viewer.

## Reproduction file

- `synthetic-chart-scroll-lag.univer`
- Workbook unit: `u-msa1sjmf-7yp544`
- 6 worksheets
- 828 formula cells
- 10 native charts
- Approximately 1.1 MB

The workbook contains synthetic labels and numbers only. It is a clean baseline with no original business content, personal paths, credentials, images, or prior document history.

## Environment

- OS: macOS (arm64)
- Node.js: v24.15.0
- npm: 11.12.1
- Univer CLI: 0.3.9 (`f3e7590`)

## Reproduce from an empty directory

```bash
mkdir univer-cli-chart-scroll-repro
cd univer-cli-chart-scroll-repro
git clone https://github.com/stan373/univer-cli-repro-chart-heavy-scroll-lag.git
cd univer-cli-repro-chart-heavy-scroll-lag

npm install -g univer-cli@0.3.9
univer --version
univer unit list synthetic-chart-scroll-lag.univer --json
univer inspect workbook synthetic-chart-scroll-lag.univer --unit u-msa1sjmf-7yp544 --json
univer open synthetic-chart-scroll-lag.univer --unit u-msa1sjmf-7yp544
```

1. Open the URL printed by `univer open`.
2. Keep the `Dashboard` worksheet active.
3. Repeatedly scroll vertically down and up inside the worksheet canvas.
4. Observe intermittent stalls while the worksheet and charts move.

## Observed result

In one 12-scroll sample, Viewer input completion times were:

```text
16, 40, 8, 18, 325, 6, 217, 35, 332, 30, 314, 6 ms
```

Four of the 12 scroll operations took more than 200 ms. The stalls are visible during ordinary trackpad or mouse-wheel scrolling.

## Expected result

Vertical scrolling should remain smooth and consistently responsive on a worksheet of this size and chart/formula density.
