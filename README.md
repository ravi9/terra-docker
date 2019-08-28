This repo provides docker images for running jupyter notebook in [Terra](https://app.terra.bio)

[terra-jupyter-base](terra-jupyter-base/README.md)

[terra-jupyter-r](terra-jupyter-r/README.md)

# Development
## Using git secrets
Make sure git secrets is installed:
```
brew install git-secrets
```
Ensure git-secrets is run: If you use the rsync script to run locally you can skip this step

```
cp -r hooks/ .git/hooks/
chmod 755 .git/hooks/apply-git-secrets.sh
```

## Generate New Image
- Update VERSION file
- Update CHANGELOG.md
- Follow [instructions](https://broadworkbench.atlassian.net/wiki/spaces/AP/pages/100401153/Testing+notebook+functionality+with+Fiab) to test the image
- Run jenkins job for publishing the image (TBD)

## Testing your image manually
- In a terminal, do `gcloud auth login` and follow the instructions to authenticate with a google account that has appropriate permissions in Terra
- Create a cluster
  - Do ```curl -X PUT --header 'Content-Type: application/json' --header "Authorization: Bearer `gcloud auth print-access-token`" -d @/tmp/createCluster https://leonardo.dsde-prod.broadinstitute.org/api/cluster/{google project}/{cluster name}```
  - /tmp/createCluster looks like
    ```
    {
        "jupyterDockerImage": "{your image url}"
    }
    ```
- Go to `https://portal.firecloud.org/`
- Create a notebook and choose the cluster you just created
- Open the notebook and check if things work as expected

## Automation Tests
[Here](https://github.com/DataBiosphere/leonardo/tree/develop/automation/src/test/scala/org/broadinstitute/dsde/workbench/leonardo/notebooks) are automation tests for various docker image, please update the image hash for relevant tests.