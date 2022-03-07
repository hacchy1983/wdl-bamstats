[![Docker Repository on Quay.io](https://quay.io/repository/collaboratory/dockstore-tool-bamstats/status "Docker Repository on Quay.io")](https://quay.io/repository/collaboratory/dockstore-tool-bamstats)

# wdl-bamstats

A fork of bamstats to focus entirely on the WDL aspect for tutorial purposes.

## Running Through the Dockstore CLI

This workflow can be found at the [Dockstore](https://dockstore.org). Login with your GitHub account and follow the directions to setup the CLI. It lets you run a Docker container with a CWL/WDL descriptor locally, using Docker and the CWL/WDL command line utility. This is great for testing.

### Make a parameters JSON

Parameterization of the workflow is done via a JSON file of key-value pairs to fill in the workflow's input variables. You can generate one using Dockstore's wdl2json feature:
```
$ dockstore workflow convert wdl2json --wdl Dockstore.wdl > Dockstore.json
$ cat Dockstore.json
{
  "bamstatsWorkflow.mem_gb": "Int",
  "bamstatsWorkflow.bam_input": "File"
}
```
The current values, `Int` and `File` in this case, tell you what type is expected. You need to fill these in the actual values you intend on using. A filled-in example is present in this repo as `test.wdl.json`

```
$ cat test.wdl.json
{
  "bamstatsWorkflow.bam_input": "rna.SRR948778.bam",
  "bamstatsWorkflow.mem_gb": "4"
}
```

#### Run with the CLI

Run it using the Dockstore CLI:

```
$ dockstore workflow launch --local-entry Dockstore.wdl --json Dockstore.json
```
You could replace `Dockstore.json` with `test.wdl.json` if you'd prefer to use the filled-in example JSON we provided, but keep in mind you will also need our example test data, rna.SRR948778.bam, and it will need to be in the same folder as test.wdl.json!

The Dockstore CLI will then call upon [Cromwell](https://github.com/broadinstitute/cromwell), which launches the WDL workflow on your local machine. In a few minutes, you should see that your workflow has successfully completed, generating a zip file.
