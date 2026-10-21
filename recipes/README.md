# vLLM Ascend recipe overlays

This directory holds the Huawei Ascend content of the model recipes that
[recipes.vllm.ai](https://recipes.vllm.ai) renders. Each recipe itself lives in
[vllm-project/vllm](https://github.com/vllm-project/vllm/tree/main/recipes/models),
which covers NVIDIA, AMD and the other platforms. The Ascend parts live here,
next to the vLLM Ascend code they describe.

- `overlays/<hf_org>/<hf_repo>.yaml` adds Ascend content to the vLLM recipe at
  the same path: Ascend variants (checkpoints whose `supported_hardware` is
  Ascend only), `meta.hardware.ascend_*` entries, `npu` or `ascend_*` keys in
  hardware-keyed maps (`hardware_overrides`, `strategy_hardware`,
  `hardware_notes`, ...), Huawei `dependencies`, and `guide_sections` filling
  the `<!-- overlay:vllm-ascend:<name> -->` markers in the vLLM guide.
- `models/<hf_org>/<hf_repo>.yaml` (optional) is a whole recipe for a model
  that runs only on Ascend: every variant's `supported_hardware` lists only
  Ascend profiles.

The renderer, [vllm-project/recipes](https://github.com/vllm-project/recipes),
merges an overlay into its recipe at build time. The merge is add-only and
limited to Huawei hardware keys: an overlay can't change what another
platform renders, and a key the vLLM recipe already sets with a different
value is an error. The vLLM recipes, in turn, may not carry Ascend content.

## Editing

Fields are defined in
[SCHEMA.md](https://github.com/vllm-project/recipes/blob/main/SCHEMA.md).
Hardware profile ids (`ascend_910c`, `ascend_950pr`, ...) come from the
renderer's
[`taxonomy.yaml`](https://github.com/vllm-project/recipes/blob/main/taxonomy.yaml);
a new Ascend profile is added there first.

To validate a change exactly as this repository's `Recipes` PR check does, run
from a checkout of `vllm-project/recipes`:

```bash
node scripts/validate-source.mjs --source vllm-ascend --dir ~/vllm-ascend/recipes
```

To preview the site with your changes, add a gitignored `sources.local.yaml`
to that checkout with `overrides: { vllm-ascend: ~/vllm-ascend/recipes }` and
run `pnpm dev`.
