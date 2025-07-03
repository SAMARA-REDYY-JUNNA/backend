@Library('jenkins-shared-library') _

// create variable of map type and set the values

def configMap = [
    type: "nodejsEKS",
    component: "backend",
    project: "expense"
]

def branch = env.BRANCH_NAME ?: ""

echo "DEBUG: Branch Name = '${branch}'"

if (!branch.toLowerCase().equals("main")) {
    pipelineDecission.decidePipeline(configMap)
} else {
    echo "Proceed with CR or NON-PROD pipeline"
}

