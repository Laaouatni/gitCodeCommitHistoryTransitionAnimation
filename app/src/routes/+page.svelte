<script lang="ts">
  import { onMount } from "svelte";

  const owner = "laaouatni";
  const repoName = "arduino-2024-2025-prove";
  const filePath = "december/componentsArduino/prova2/prova2.ino";

  const apiUrlAllCommits = `https://api.github.com/repos/${owner}/${repoName}/commits?path=${filePath}`;

  let allShaCommitCodes: string[] = []
  let allRawFileCodes: string[] = []

  type TypeApiUrlAllCommits = {
    sha: string
  };

  onMount(async () => {
    const fetchAllCommits = await fetch(apiUrlAllCommits);
    const commitsData: TypeApiUrlAllCommits[] = await fetchAllCommits.json();
    const allShaCommitCodes = commitsData.map((thisCommit: TypeApiUrlAllCommits) => thisCommit.sha);
    const allRawUrl = allShaCommitCodes.map((thisShaCode) => `https://raw.githubusercontent.com/${owner}/${repoName}/${thisShaCode}/${filePath}`);
    const allRawCodeFileStrings = Promise.all(allRawUrl.map(async (thisRawUrl) => {
      const fetchRawFile = await fetch(thisRawUrl);
      return await fetchRawFile.text();
    }));
    allRawFileCodes = await allRawCodeFileStrings;

    console.log(allRawFileCodes);
  });

</script>
