# Adapter Directory

Each adapter explains how to install the shared Skill and map its role contract to one host. Read only the adapter for the selected host.

The host profile in skills/closed-loop-subagents/assets/config.example.yaml separates model identifiers and controls by platform. Keep personal settings out of the public repository. Store them in the host's user-level configuration when supported.

Do not mark an adapter fully supported until its release checks pass for Skill discovery, independent Planner/Executor/Reviewer dispatch, per-role model assignment, model upgrade routing, runtime observability reporting, and first-run configuration.
