# RPMs

## extract RPMs without installation

```shell
brew install rpm2cpio

rpm2cpio bla.rpm | cpi -idmv
# creates folder structure of rpm without installing
```

## create RPM

Ruby fpm (Effing Package Management)

```shell
fpm.ruby -s -dir -t rpm -n "RPM_NAME" -v "RPM_VERSION" -p "RPM_PATH" --description "DESCRIPTION" --prefix "DESTINATION_PATH" -C "DESTINATION_PATH"

# chatgpg explanation

    # -s: Specifies the source of the package to be built. It is likely followed by an argument that specifies the type of source (e.g., "dir" for a directory).

    # -dir: Indicates that the package source is a directory. This option is typically used in conjunction with the -s option.

    # -t rpm: Specifies the target package format to be generated, which, in this case, is an RPM (Red Hat Package Manager) package.

    # -n "RPM_NAME": Specifies the name of the RPM package being created. Replace "RPM_NAME" with the desired name.

    # -v "RPM_VERSION": Specifies the version of the RPM package being created. Replace "RPM_VERSION" with the desired version.

    # -p "RPM_PATH": Specifies the path where the generated RPM package should be saved. Replace "RPM_PATH" with the desired file path.

    # --description "DESCRIPTION": Sets the description for the RPM package. Replace "DESCRIPTION" with the desired package description.

    # --prefix "DESTINATION_PATH": Specifies the prefix or base directory for the files contained within the package. Replace "DESTINATION_PATH" with the desired path.

    # -C "DESTINATION_PATH": Changes to the specified directory before processing the package. This option is used to ensure that the correct files are included in the package. Replace "DESTINATION_PATH" with the desired path.
```
