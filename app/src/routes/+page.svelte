<script lang="ts">
  import { onMount } from "svelte";

  const owner = "laaouatni";
  const repoName = "arduino-2024-2025-prove";
  const filePath = "december/componentsArduino/prova2/prova2.ino";

  const apiUrlAllCommits = `https://api.github.com/repos/${owner}/${repoName}/commits?path=${filePath}`;

  let allShaCommitCodes: string[] = $state([]);
  let allRawFileCodes: string[] = $state([])

  type TypeApiUrlAllCommits = {
    sha: string
  };

  onMount(() => {
    fetch(apiUrlAllCommits)
      .then((response) => response.json())
      .then((data: TypeApiUrlAllCommits[]) => {
        data.forEach((thisCommit) => {
          allShaCommitCodes = [...allShaCommitCodes, thisCommit.sha];
        });
      }).then(() => {
        allShaCommitCodes.forEach((thisShaCode) => {
          const rawUrl = `https://raw.githubusercontent.com/${owner}/${repoName}/${thisShaCode}/${filePath}`;
          fetch(rawUrl)
            .then((response) => response.text())
            .then((thisVersionCode) => {
              allRawFileCodes = [...allRawFileCodes, thisVersionCode];
            });
        });
      }).then(() => {
        console.log(allRawFileCodes);
      })
  });
</script>
