<script lang="ts">
  const owner = "laaouatni";
  const repoName = "arduino-2024-2025-prove";
  const filePath = "december/componentsArduino/prova2/prova2.ino";

  const apiUrlAllCommits = `https://api.github.com/repos/${owner}/${repoName}/commits?path=${filePath}`;

  let shaCommitVersionCodes: string[] = [];

  type TypeApiUrlAllCommits = {
    sha: string
  };

  fetch(apiUrlAllCommits)
    .then((response) => response.json())
    .then((data: TypeApiUrlAllCommits[]) => {
      data.forEach((thisCommit) => {
        shaCommitVersionCodes.push(thisCommit.sha);
      });
    });
</script>
