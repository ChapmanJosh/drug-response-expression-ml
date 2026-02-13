# Data layout (local-only)

This project keeps datasets OUT of Git.

Place files like:
- data/raw/      original downloaded data (read-only)
- data/processed/ cleaned/merged tables used for modeling
- data/external/  reference tables / metadata

Keep only small documentation files in Git (like this README).
