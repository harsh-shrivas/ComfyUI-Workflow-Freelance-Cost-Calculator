# ComfyUI-Workflow-Freelance-Cost-Calculator

An automated multimodal invoice calculation and rate sheet auditing workflow for ComfyUI.

Designed for post-production supervisors, agency leads, and freelance VFX artists, this workflow uses **Google Gemini Vision** (`gemini-3.8-flash`) to visually parse production logs, rate cards, and screenshot manifests. It extracts artist names, project line items, and durations, applies dynamic pricing classification (tier/slab rates or continuous per-minute rates), computes total billing mathematics, and flags missing or incomplete entries.

---

## Workflow Architecture

1. **Visual Sheet Ingestion (`LoadImage`):** Ingests screenshot rate sheets, export tables, or delivery manifests directly into the graph.
2. **Accounting Directive Engine (`PrimitiveStringMultiline`):** Directs the multimodal vision model through a 4-step auditing protocol:
   - **Data Extraction & Validation:** Identifies artist header, individual deliverable titles, duration timestamps (`mm:ss`), and format tags (`(PDF)`, `3D`, `VFX`).
   - **Dynamic Rate Classification:** Differentiates between tier-based slab pricing (e.g., `1:11 to 2:10`) and continuous per-minute rates, routing specific tags (like dedicated PDF delivery rates) to their respective fee charts.
   - **Mathematical Computation:** Sums item fees, computes grand billable duration, and calculates the total payable cost.
   - **Validation & Exception Handling:** Automatically flags empty, dash (`-`), or unlisted durations as `MISSING` with a `₹0` subtotal to prevent erroneous billing.
3. **Multimodal Analysis (`GeminiAPI`):** Processes the image and logic directives using `gemini-3.8-flash` with structured text output.
4. **Interactive Audit Canvas (`ShowText`):** Displays a formatted billing summary and status log ready for finance approval and payout records.

---

## Prerequisites & Required Nodes

Install the following custom nodes via ComfyUI Manager:

- **ComfyUI-Gemini:** Provides `GeminiAPI` multimodal integration.
- **ComfyUI-Custom-Scripts:** Provides `ShowText|pysssss` display nodes.
- **ComfyUI-VideoHelperSuite (VHS):** Optional helper nodes for frame and duration inspection.

---

## Setup & Configuration

1. Drag and drop `Freelance Cost Calculator.json` directly onto your ComfyUI workspace.
2. In the **GeminiAPI** node:
   - Insert your Gemini API Key in `api_key`.
   - Set the model to `gemini-3.8-flash` or any active vision-capable Gemini model.
3. In the **LoadImage** node, load or paste the image of your rate card or deliverable tracker.
4. Click **Queue Prompt**. The calculated breakdown and invoice total will populate in the `Final Invoice` preview window.

---

## License

MIT License. Free to use, adapt, and integrate into internal studio accounting and automated production pipelines.
