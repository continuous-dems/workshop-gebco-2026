---
title: "6 - Day 2 Introduction and IVERT"
---

Day 2 builds on the Fiji test DEM and YAML walkthrough from Day 1. Now that participants have seen how a complete DEM recipe is structured, the goal is to step through the main workflow tools more directly and then run a custom region using `globato`.

By the end of Day 2, participants should understand how `fetchez`, `transformez`, `globato`, and IVERT fit together in a reproducible coastal DEM workflow. Participants should be able to discover source data, understand vertical transformation needs, run or modify a recipe, inspect generated DEM outputs, and consider basic validation and quality-control questions.

## Learning Goals

By the end of this section, participants should be able to:

- Reactivate the `cudem` environment
- Confirm that the main CUDEM tools are still working
- Understand the role of `fetchez` in data discovery and source-data access
- Understand the role of `transformez` in vertical harmonization
- Understand how `globato` connects the CUDEM workflow
- Run or inspect a custom DEM recipe with `globato`
- Understand the role of `ivert` in independent DEM validation
- Review DEM outputs and identify basic quality-control issues
- Understand how a Fiji-style workflow can be adapted to another coastal region

---

## Reactivate the CUDEM Environment

Open a Miniforge Prompt or terminal and reactivate the workshop environment:

```bash
conda activate cudem
```

Confirm that the main tools are available:

```bash
fetchez --help
transformez --help
globato --help
ivert --help
```

If these commands return help text, the environment is ready for the Day 2 exercises.

---

## Independent DEM Validation with IVERT

> **Placeholder — this section will be completed in the next part of the workshop.**

`ivert` uses NASA's ICESat-2 altimetry data to independently validate DEM elevations against a reference dataset that was not used to build the DEM.

Content to be added here will cover:

- The purpose of independent DEM validation and where it fits in the CUDEM workflow
- Required inputs and expected outputs
- Running the Fiji test DEM through `ivert`
- Filtering and interpreting the validation results
- Setting up custom validations for your own DEMs
