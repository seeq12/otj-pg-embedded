# Seeq's build of otj-pg-embedded

This fork is published as `com.opentable.components:otj-pg-embedded:<upstream version>-SEEQ`. It adds to upstream:

- `Builder.setUsername` and `setPassword`, for the role that the container creates.
- `Builder.copyToContainer`, which copies files into the container before it starts, for where a bind mount cannot
  reach them.

## Moving to a new upstream release

Branch `seeq/<version>` from the upstream tag `otj-pg-embedded-<version>`, cherry-pick the Seeq commits of the previous
`seeq/` branch and set the version in `pom.xml` to `<version>-SEEQ`.

## Publishing

Build and test it (this needs Docker), then upload it to the `maven-releases` repository:

```bash
mvn verify
sq secret exec NEXUS_USERNAME,NEXUS_PASSWORD -c 'mvn -s .mvn/seeq-nexus-settings.xml deploy:deploy-file \
    -DrepositoryId=seeq-nexus -Durl=https://nexus.seeq-labs.com/repository/maven-releases \
    -DpomFile=pom.xml -Dfile=target/otj-pg-embedded-1.1.3-SEEQ.jar \
    -Dsources=target/otj-pg-embedded-1.1.3-SEEQ-sources.jar'
```

Plain `mvn deploy` would publish to Maven Central instead, as upstream's release does.
