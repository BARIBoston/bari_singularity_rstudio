# bari_singularity_rstudio

This repository contains the source code for the BARI Singularity image, containing R packages for geospatial analysis. The full documentation for Singularity can be found on the [syslabs.io documentation site](https://sylabs.io/guides/3.5/user-guide/index.html).

## Configuring installed packages

The complete image build process is outlined in the [Singulariy definition file](Singularity). Most likely, you will want to edit the `PACKAGES` array in the `%post` section of the definition file.

For full documentation, see the [The Definition File](https://sylabs.io/guides/3.5/user-guide/definition_files.html) section of the syslabs.io documentation site.

## Building

To quickly build the BARI Singularity image, run the following command:

```bash
singularity build bari.img Singularity
```

When prototyping, though, it will likely be more useful to use a temporary sandbox directory to iterate through commands before writing the final procedure to the Singularity definition file:

```bash
singularity build --sandbox bari-dev/ Singularity
```

You can then access a root shell that will persist changes as follows:

```bash
sudo singularity shell --writable bari-dev/
```

Once you are finished prototyping, it is best to edit the definition file, but if you are impatient, you can build an image directly from the temporary sandbox:

```bash
sudo singularity build bari.img bari-dev/
```

For full documentation, see the [Building a container](https://sylabs.io/guides/3.5/user-guide/build_a_container.html) section of the syslabs.io documentation site.
