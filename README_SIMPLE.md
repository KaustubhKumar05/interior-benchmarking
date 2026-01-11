# Kitchen Vision AI Benchmark

A tool that tests how well different AI models can analyze kitchen images.

## What Does This Do?

This system shows AI models pictures of kitchens and asks them to identify:
- Cabinet types (base, wall, tall, loft)
- Materials and finishes
- Colors
- Hardware (handles, knobs)
- Lighting fixtures

It then scores how accurately each AI model describes what it sees compared to the correct answers.

## How Scoring Works

**Scores are percentages from 0% to 100%**
- **100%** = Perfect match with the correct answer
- **0%** = Completely wrong

### What Gets Scored

Each kitchen image is analyzed for 7 components:

1. **Base Cabinets** (15%) - Lower cabinets below the counter
2. **Wall Cabinets** (15%) - Upper cabinets on the wall
3. **Tall Cabinets** (15%) - Full-height units (pantries, etc.)
4. **Loft Cabinets** (15%) - Highest cabinets near the ceiling
5. **Dado/Backsplash** (5%) - Wall covering behind counters
6. **Floor** (5%) - Flooring material
7. **Colors** (20%) - How accurately colors are identified
8. **Handles** (10%) - Hardware type and finish

### For Each Component, We Check:

- **Did the AI see it?** (30%) - Is the component detected?
- **Right material?** (20%) - Correct finish (laminate, wood, etc.)
- **Right colors?** (30%) - How close are the colors?
- **Right hardware?** (20%) - Correct handle type and finish

### Color Matching

Colors are scored using human perception:
- **Identical colors** = 100%
- **Slightly different** = High score (hard to notice)
- **Completely different** = 0%

The system uses LAB color space, which matches how humans actually see color differences.

## Reading the Report

### Summary Table

Shows the most important info for each AI model:

```
| Model | Avg Score (%) | Samples | Total Cost | Avg Cost/Request | Time |
```

- **Avg Score**: Overall accuracy across all test images
- **Samples**: Number of kitchen images tested (usually 10)
- **Total Cost**: How much the AI model charged
- **Avg Cost/Request**: Cost per image analyzed
- **Time**: How long the test took

### Component Performance Table

Shows which parts of the kitchen each AI is best at analyzing. **Models are ranked from best to worst overall.**

Example:
```
| Model    | Base  | Wall  | Tall  | Loft  | Dado  | Floor | Ceiling |
| Model A  | 77%   | 73%   | 71%   | 78%   | 80%   | 79%   | 59%     |
| Model B  | 69%   | 71%   | 52%   | 48%   | 63%   | 76%   | 52%     |
```

This tells you:
- Model A is better overall
- Model A excels at identifying dado/backsplashes (80%)
- Model B struggles with loft cabinets (48%)

### What Makes a Good Score?

- **90-100%**: Excellent - nearly perfect identification
- **70-89%**: Good - accurate with minor errors
- **50-69%**: Fair - gets main features but misses details
- **Below 50%**: Poor - significant errors in identification

## Quick Start

1. Get an API key from [OpenRouter](https://openrouter.ai/keys)
2. Add it to a `.env` file
3. Choose which AI models to test in `config.yaml`
4. Run: `uv run python -m src.benchmark`
5. Check the report in `results/{timestamp}/report.md`

## Understanding Costs

- **Free models** show "N/A" for costs (no charge)
- **Paid models** charge per image analyzed
- Costs vary widely - from fractions of a cent to several cents per image
- The report shows both total cost and average cost per request

## Why This Matters

Different AI models have different strengths:
- Some are better at colors
- Some are better at detecting small details like handles
- Some are faster but less accurate
- Some are expensive but very precise

This benchmark helps you pick the right AI model for your needs based on accuracy, speed, and cost.

## Technical Setup

For developers who want to run or modify this:

### Requirements
- Python 3.11+
- UV package manager
- OpenRouter API key

### Installation
```bash
# Install UV
curl -LsSf https://astral.sh/uv/install.sh | sh

# Install dependencies
uv sync

# Configure API key
cp .env.example .env
# Edit .env and add your OpenRouter API key

# Run benchmark
uv run python -m src.benchmark
```

### Configuration

Edit `config.yaml` to:
- Add/remove AI models to test
- Adjust scoring weights
- Change parallel request settings

Browse available AI models at [OpenRouter Models](https://openrouter.ai/models)

## Files Generated

Each benchmark run creates:
- `results/{timestamp}/report.md` - The main report
- `results/{timestamp}/{model_name}/` - Individual AI responses for manual review

---

*For detailed technical documentation, see the original README.md*
