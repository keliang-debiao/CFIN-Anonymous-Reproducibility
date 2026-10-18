CFIN Anonymous Reproducibility Package
This repository contains the source code, configuration files, workflow scripts, tests, and supporting documentation for Cross-Feature Interaction Networks (CFIN) for student dropout prediction. The package includes implementations for data preparation, feature grouping, model training, baseline comparison, prediction-horizon analysis, sensitivity analysis, ablation studies, statistical comparison, feature attribution, efficiency measurement, and report generation.
Repository Contents
Path	Contents
src/cfin/	Python modules implementing the models, data pipeline, experiment workflow, and analysis utilities.
configs/	Shared protocol settings, dataset-specific definitions, and hyperparameter search spaces.
scripts/	Entry-point scripts for data acquisition, data registration, study execution, verification, timing, testing, and checksum generation.
tests/	Unit and protocol tests for data handling, model behavior, metrics, and statistical utilities.
data/raw/	Destination directory for registered source data; the archive contains a directory placeholder.
data/processed/	Reserved directory for processed data; the archive contains a directory placeholder.
results/	Destination directory for artifacts produced by experiment and analysis workflows; the archive contains a directory placeholder.
manuscript_targets/	Reference CSV files organized by analysis category, with a README explaining their role.
logs/	Pipeline-verification and test-run logs, together with a README describing their provenance and scope.
docs/	Reserved documentation directory.


Model Implementations
The package contains CFIN and the following comparison models:
- Logistic regression.
- Random forest.
- XGBoost.
- TabNet.
- FT-Transformer.
The CFIN implementation includes numerical and categorical feature tokenization, feature-identity embeddings, group-level Transformer encoders, an inter-group context encoder, and a classification head. The neural-model code also includes a weighted focal-loss implementation and an FT-Transformer implementation with a classification token.
Conventional estimators are constructed in src/cfin/baselines.py. TabNet is integrated through the experiment workflow using the pytorch-tabnet dependency.
Core Python Modules
File	Functionality
src/cfin/model.py	Feature tokenization, CFIN and FT-Transformer architectures, weighted focal loss, and neural-model construction.
src/cfin/data.py	Dataset loading, field and label normalization, data cleaning, dataset validation, fixed partitioning, prediction-horizon filtering, numerical preprocessing, categorical encoding, and baseline input preparation.
src/cfin/groups.py	Feature-to-group assignment using configured field names and patterns, together with randomized grouping utilities.
src/cfin/train.py	Array-backed datasets, neural-model fitting, device selection, and probability prediction.
src/cfin/experiment.py	Individual experiment orchestration, model selection, prediction export, configuration recording, and run-event recording.
src/cfin/tuning.py	Hyperparameter suggestions, training-partition cross-validation, and Optuna study management.
src/cfin/metrics.py	Binary-threshold selection and classification metrics, including accuracy, precision, recall, F1, ROC-AUC, per-class summaries, and confusion matrices.
src/cfin/statistics.py	Exact McNemar testing, paired DeLong testing, paired stratified bootstrap comparisons, and Holm adjustment.
src/cfin/interpret.py	Permutation SHAP attribution and feature- and group-level attribution exports.
src/cfin/ablation.py	Architecture, loss-function, and feature-grouping ablation workflows and aggregation utilities.
src/cfin/efficiency.py	Model inference benchmarking utilities.
src/cfin/reporting.py	Aggregation of run records, horizon analyses, sensitivity analyses, and attribution records into CSV tables, plus comparison with reference CSV files.
src/cfin/config.py	YAML loading, configuration merging, project-root resolution, and configuration export.
src/cfin/utils.py	Random-seed handling, JSON input/output, timestamps, file hashes, package inventories, and hardware records.
src/cfin/cli.py	Command-line interface for individual runs, tuning, statistical comparison, attribution, reporting, and reference-file comparison.


Configuration Files
File	Contents
configs/common.yaml	Shared partitioning and evaluation settings, training options, architecture parameters, threshold settings, attribution settings, and efficiency-measurement settings.
configs/inels.yaml	INELS data location, source reference, target definition, validation expectations, excluded identifiers, missing-value and score-cleaning rules, feature horizons, continuous fields, feature groups, and class weights.
configs/uci.yaml	UCI student dropout and academic success data location, source reference, target definition, validation expectations, prediction horizons, continuous fields, feature groups, and class weights.
configs/tuning.yaml	Model-specific hyperparameter search spaces for CFIN and the comparison models.


Dataset configurations cover binary and multiclass prediction. They also define feature-availability settings for enrollment, semester-based prediction, score exclusion, and score-retention sensitivity analysis, as applicable to each dataset. Source datasets are obtained separately rather than bundled in the archive.
Workflow Scripts
File	Purpose
scripts/fetch_data.py	Data acquisition entry points for the configured UCI and INELS sources.
scripts/register_data.py	Registration and validation of locally supplied dataset files.
scripts/run_study.py	Dispatch of tuning, training, horizon analysis, sensitivity analysis, ablation, statistical comparison, attribution, efficiency measurement, and reporting stages.
scripts/run_efficiency.py	Efficiency-measurement workflow for configured models and saved run artifacts.
scripts/verify_pipeline.py	Fast pipeline checks using an in-memory validation fixture.
scripts/run_tests_with_log.py	Execution of the test suite with captured output written to a log file.
scripts/check_repository.py	File-name and text checks, Python syntax checks, and required-file checks.
scripts/build_manifest.py	Generation of the SHA-256 file manifest.


The ablation code includes conditions for removing inter-group context, using a flat Transformer, changing the loss function, and replacing semantic feature groups with random, correlation-based, or alternative domain groups.
Tests and Verification Logs
File	Coverage
tests/test_data.py	Field-name normalization and correction before partitioning.
tests/test_model.py	Neural-model forward-pass dimensions and loss calculation.
tests/test_metrics.py	Metric bounds and threshold tie handling.
tests/test_statistics.py	McNemar symmetry, Holm-adjustment ordering, and paired DeLong behavior.
tests/conftest.py	Test import-path configuration.


logs/pipeline_verification.log records the fast validation workflow. logs/test_run.log records test-suite execution. logs/README.md describes the provenance and scope of these records. These logs concern repository verification rather than full-study experiment outputs.
Reference Files and Output Locations
manuscript_targets/ contains the following reference files:
- performance.csv
- horizons.csv
- ablation.csv
- statistics.csv
- feature_attribution.csv
- group_attribution.csv
- efficiency.csv
- README.md
These CSV files hold manuscript reference values and are separate from generated experiment artifacts. The reporting module includes a comparison utility for reference and generated tables.
The experiment workflow contains writers for predictions, metrics, resolved configurations, model checkpoints, run logs, data hashes, software inventories, and hardware records. The analysis workflows contain exporters for tuning records, statistical comparisons, attribution tables, efficiency records, and aggregated CSV tables under results/.
Environment and Packaging Files
- pyproject.toml: package metadata, dependencies, the cfin command-line entry point, and pytest configuration.
- requirements.txt: pinned Python dependencies.
- environment.yml: Conda environment definition.
- Dockerfile: container environment definition.
- .dockerignore: container-build exclusion rules.
- Makefile: named workflow targets for installation, verification, testing, and study stages.
- .github/workflows/verify.yml: GitHub Actions workflow for repository verification and tests.
- .gitignore: version-control exclusion rules.
The declared dependencies include PyTorch, NumPy, pandas, scikit-learn, SciPy, XGBoost, PyTorch TabNet, Optuna, SHAP, PyYAML, pytest, and data-acquisition utilities.
Supporting Documentation and Metadata
File	Contents
DATA.md	Data-source references, expected input structure, preprocessing definitions, validation rules, and registration-record fields.
REPRODUCIBILITY.md	Partitioning, tuning, imbalance handling, threshold selection, evaluation, attribution, timing, and artifact-provenance conventions.
AI_RUNBOOK.md	Execution order, artifact requirements, and discrepancy-handling instructions for automated experiment workflows.
ANONYMITY.md	Anonymous-review preparation guidance.
CITATION.cff	Software citation metadata.
MANIFEST.sha256	File-integrity checksums for the supplied archive contents.
LICENSE	MIT license for the repository code.


Dataset usage terms remain those of the respective source providers.
