0) Prereqs for archery:
```
sudo apt-cache update
sudo apt-get install python3-pip
pip3 install pygit2 --only-binary pygit2

wget https://files.pythonhosted.org/packages/73/00/b78f9fae05bb1633f7209aa394fa0c3563ef760ab7f47ac37768bf4e4d78/pyOpenSSL-23.0.0-py3-none-any.whl
sudo python3 -m easy_install pyOpenSSL-23.0.0-py3-none-any.whl

# Maybe you may need to do this as well:
sudo ln -s /usr/libexec/docker/cli-plugins/docker-compose /usr/bin/docker-compose
```

1) Then install archery:
```
pip3 install -e "dev/archery[all]"
```
  - Incase you need the documentation for other setups: https://arrow.apache.org/docs/developers/continuous_integration/archery.html

2) Then run:
  $HOME/.local/bin/archery docker run java-jni-manylinux-2014

3) Then to install all of these jars to your local maven repo, run (while you are in the arrow dir):

```
find java-dist/ \( -name "*.jar" -o -name "*.pom" \) |
  grep -v "\-javadoc" | grep -v "\-sources" | grep -v "\-tests" |
    sed 's#\(java-dist/\)\(.*\)\(-\)\(10.*\)\(\.\)\(.*\)#mvn install:install-file -Dfile="\1\2\3\4\5\6" -DgroupId=org.apache.arrow -DartifactId=\2 -Dversion=\4 -Dpackaging=\6#g' |
      xargs -I{} bash -c "{}"
```
