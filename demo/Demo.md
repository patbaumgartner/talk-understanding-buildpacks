# Demos

## Important Note

Make sure containerd for WASM is not enabled. Otherwise some buildpacks might fail.

All commands below are run from the `demo/` directory of this repository.

## Install Pack CLI

```bash
sudo add-apt-repository ppa:cncf-buildpacks/pack-cli
sudo apt-get update
sudo apt-get install pack-cli
```

## Update Docker Images

```bash
docker images --format "{{.Repository}}:{{.Tag}}" | xargs -n1 docker pull
docker image prune -af
```

## Demo 1: First Builds with Different Builders

```bash
pack builder suggest
```

```bash
cd my-app
pack build my-app-google --builder gcr.io/buildpacks/builder
```

```bash
cd my-app
pack build my-app-noble --builder paketobuildpacks/builder-noble-java-tiny
```

## Demo 2: Custom Base Images and Builder

```bash
cd base-images
./build.sh -f patbaumgartner/buildpack-base noble
cd -

cd builders/noble
pack builder create patbaumgartner/buildpack-builder:noble --config builder.toml
cd -

cd my-app
pack build my-app-patbaumgartner-noble --builder patbaumgartner/buildpack-builder:noble
```

## Demo 3: SBOM

```bash
cd my-app
pack sbom download my-app-patbaumgartner-noble --output-dir ../my-app-patbaumgartner-noble-sbom

cat ../my-app-patbaumgartner-noble-sbom/layers/sbom/launch/paketo-buildpacks_executable-jar/sbom.cdx.json | \
  jq '.components[] | select(.name=="jackson-databind")'
```

## Demo 4: Spring Boot Maven Plugin

```bash
cd spring-petclinic
./mvnw spring-boot:build-image -Dspring-boot.build-image.imageName=my-petclinic-app
```

## Demo 5: AOT Cache with Java 25

```bash
cd spring-petclinic-aot-cache-25
./mvnw spring-boot:build-image -Dspring-boot.build-image.imageName=my-petclinic-aot
docker run --rm -p 8080:8080 my-petclinic-aot
```

## Demo 6: Running the License-Checker Buildpack

```bash
pack build my-app --path ./my-app \
    --builder paketobuildpacks/ubuntu-noble-builder \
    --buildpack paketo-buildpacks/java \
    --buildpack ./buildpacks/license-checker \
    --verbose
```

Alternatively, use the newest Ubuntu 26.04 LTS builder (`paketobuildpacks/ubuntu-resolute-builder`).

### Debugging with FOSSA

```bash
pack build my-app --path ./my-app \
    --builder paketobuildpacks/ubuntu-noble-builder \
    --buildpack paketo-buildpacks/java \
    --buildpack ./buildpacks/license-checker \
    --env BP_DEBUG_FOSSA=true \
    --verbose
```

## Demo 7: Rebase Instead of Rebuild

```bash
# Patch the OS layer without rebuilding the app (re-pulls the recorded run image)
pack rebase my-app-noble

# Or move to an explicitly newer run image tag
pack rebase my-app-noble --run-image paketobuildpacks/ubuntu-noble-run-tiny:latest --force
```