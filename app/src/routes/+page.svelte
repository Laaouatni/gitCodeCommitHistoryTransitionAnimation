<script lang="ts">
  import { onMount } from "svelte";

  const owner = "laaouatni";
  const repoName = "arduino-2024-2025-prove";
  const filePath = "december/componentsArduino/prova2/prova2.ino";

  const apiUrlAllCommits = `https://api.github.com/repos/${owner}/${repoName}/commits?path=${filePath}`;

  let shaCommitVersionCodes: string[] = $state([]);

  type TypeApiUrlAllCommits = {
    sha: string
  };

  onMount(() => {
    fetch(apiUrlAllCommits)
      .then((response) => response.json())
      .then((data: TypeApiUrlAllCommits[]) => {
        data.forEach((thisCommit) => {
          shaCommitVersionCodes = [...shaCommitVersionCodes, thisCommit.sha];
        });
      })
  });

  $effect(() => {
    console.log(shaCommitVersionCodes);
  });
</script>
