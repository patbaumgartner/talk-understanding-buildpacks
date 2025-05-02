# Sample Alpine Builder

## Usage

### Creating the Builder

```bash
pack builder create patbaumgartner/buildpack-builder:alpine --config builder.toml
```

#### Build App with Builder

```bash
pack build my-app --builder patbaumgartner/buildpack-builder:alpine --path ../my-app
```

After building the app you should be able to simply run it via `docker run -it -p 8080:8080 my-app`.
Go to [localhost:8080](http://localhost:8080) to see the app running.
