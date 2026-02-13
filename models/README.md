# Models (local-only)

Trained model artifacts are NOT tracked in Git:
- .pkl, .joblib, .onnx, .pt, etc.

When publishing to GitHub later, consider:
- saving model artifacts as GitHub Releases, or
- re-training from scripts in CI, or
- logging artifacts with MLflow (and keeping the run metadata only).
