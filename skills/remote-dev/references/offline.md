# Offline Preparation and Downloads

Use this reference when the requested environment setup or file download is affected by restricted internet access or slow download sources. Reuse a working remote environment unless the user asks to change it.

## Match the remote target

Prepare files for the environment that will consume them. For dependencies, establish the relevant remote operating system, CPU architecture, Python version, and GPU or CUDA requirements. Collect only what matters for the requested operation; do not make every task run a complete environment audit.

Do not copy the local virtual environment as a replacement for the remote environment. Lock files specify dependencies but do not contain the dependency files themselves. Prepare target-compatible packages and any required build dependencies when offline installation is needed. If a package must be built, choose a compatible build environment or use the remote toolchain with its dependencies available.

For model tasks, include the required weights, configuration, tokenizer, and other assets at the intended version. Check for downloads deferred until first use rather than assuming installed packages are sufficient.

## Use the available network path

If the remote can download the required files and the user permits it, use an appropriate source directly. If it cannot, prepare the files on the connected local machine, sync them to a suitable remote location, and use them in the intended remote environment. Separate these files from routine source synchronization when needed.

For slow downloads, compare candidate sources or mirrors when that would help. Use representative transfers of equivalent content rather than treating ping latency as download speed. Preserve the requested versions and package variants, and check available integrity information. Do not assume one mirror is always fastest or change global package-manager settings for a one-off transfer.

Use the project's existing download and installation tools. Choose offline or local-file options appropriate to those tools; do not introduce a proxy service, container workflow, or package-management framework merely to support the skill.

## Verify the requested operation

Check that transferred files are complete and usable by the intended remote environment. Run a small relevant installation or runtime check before relying on the prepared files. A successful local download does not establish that the remote task works offline.

If execution still attempts a blocked download, identify the missing file or dependency and prepare that item. Keep the diagnosis specific to the task rather than expanding it into an unrelated environment rebuild.
