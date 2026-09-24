# portability-config

Signed configuration **data** (never code) read by the browser extension at runtime.

- `webdiet.source.json` — the editable values. Everything only *adds* to what the extension already ships with.
- `webdiet.json` — the published, signed envelope `{ payload, signature }` (ECDSA P-256 / SHA-256). Generated; do not edit by hand.

Publish from the extension repository: `node scripts/dev-tools/remote-config.mjs publish` (bumps the version, signs, verifies with the key embedded in the extension, commits and pushes).

## Fields (all optional)

```jsonc
{
  "routes": { "sections": { "anamnesis": { "route": "carregarConsultas.php", "tipo": "anamnese" } } },
  "labels": {
    "sections": { "anamnesis": ["Anamnese geral"] },          // extra visible labels per section
    "libraries": { "alimentos": ["Meus alimentos"] },          // extra labels per library tab
    "viewWithoutConsultation": ["Apenas visualizar"]           // extra labels for "open without a consultation"
  },
  "safety": {                                                   // extra reviewed SHA-256 fingerprints
    "foods": { "functions": { "novaReceita": ["<sha256>"] }, "retire": ["removedFunction"],
               "editorEvents": ["<sha256>"], "modalEvents": ["<sha256>"], "formEvents": ["<sha256>"] },
    "supplements": { "functions": {}, "editorEvents": [], "modalEvents": [] }
  },
  "formats": { "anthropometryOptional": ["fieldName"], "calculationOptional": ["fieldName"] },
  "pause": { "message": "Text shown instead of starting a new copy." }
}
```

Sections: `anamnesis`, `records`, `diets`, `supplementation`, `anthropometry`, `exams`, `calculations`, `profile`, `followup`.
