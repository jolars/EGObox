---
name: egobox-egor-tuning
description: >
  Use this skill whenever the user is working with the Egor optimizer for
  Bayesian optimization. Triggers on requests to tune Egor parameters, diagnose
  optimization issues, or configure Egor for specific problem types (high-dimensional,
  constrained, parallel, expensive objectives, etc.).
---

# EGOR Tuning Skill

## Goal

Provide practical guidance for selecting and adapting EGOR optimization parameters based on:
- problem dimension
- objective evaluation runtime  
- available optimization budget
- parallel evaluation availability
- convergence progress
- constraint complexity

## Cookbook Reference

For detailed parameterization recipes with complete examples, see the **[EGObox Cookbook](../../website/content/cookbook.md)**.

The cookbook contains 11 practical recipes covering:
- Cheap vs. expensive objectives
- High-dimensional problems (d > 10, d > 50)
- Parallel/batch evaluations
- Stagnation recovery
- Constraint handling (various forms)
- Cheap constraints
- Warm restarts from existing DOE

---

## Quick Reference

### Dimension Guidelines

| Dimension | Strategy |
|-----------|----------|
| d < 10 | Standard Egor with adequate DOE |
| d > 10 | Enable KPLS (kpls_dim ≈ d/2) |
| d > 50 | Enable CoEGO with cooperative groups |

### Evaluation Cost Guidelines

| Cost | DOE Size | Iterations |
|------|----------|------------|
| Cheap | Large (3×n_dims) | High (50+) |
| Expensive | Small (n_dims+1) | Moderate (20-30) |

### Convergence Issues

If optimization stagnates:
1. Enable TREGO trust-region framework
2. Switch kernel from SquaredExponential to Matern52
3. Try different infill strategy (WB2, EI instead of LOG_EI)
4. Increase exploration via infill parameters

### Parallel Execution

When parallel evaluations are available:
- Use `QEiConfig` with appropriate batch size
- Batch size ≈ dimension/10 is a good starting point
- Strategy `KB` works well for most cases

---

## Examples

See [examples directory](examples/) for concrete use cases:
- `cheap_function.yaml` - Low-cost objective optimization
- `expensive_function.yaml` - High-cost objective optimization  
- `high_dimensional.yaml` - Problems with d > 10
- `parallel.yaml` - Batch/parallel evaluation setup
- `bad_progress.yaml` - Stagnation recovery strategies

---

## API Reference

For complete parameter definitions, see the [Python API documentation](../../website/content/python-api.md).
