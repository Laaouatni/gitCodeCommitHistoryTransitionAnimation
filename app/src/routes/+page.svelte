<script lang="ts">
  const repos = [
    {
      owner: "laaouatni",
      repoName: "arduino-2024-2025-prove",
      filePath: "/progettoLearningNovembre/laaEsp01/laaEsp01.ino"
    },
    {
      owner: "laaouatni",
      repoName: "arduino-2024-2025-prove",
      filePath: "december/componentsArduino/prova2/prova2.ino"
    },
  ];

  (async () => {
    const allFiles = await Promise.all(
      repos.map(async (repo) => {
        return await fetchGithubAllCommitFiles(repo.owner, repo.repoName, repo.filePath);
      })
    ).then((allFiles) => {
      return allFiles.flat();
    });
  
    console.log(allFiles);
  })();

  async function fetchGithubAllCommitFiles(
    owner: string,
    repoName: string,
    filePath: string,
  ) {
    type TypeApiUrlAllCommits = { sha: string };
    const apiUrlAllCommits = `https://api.github.com/repos/${owner}/${repoName}/commits?path=${filePath}`;
    const fetchAllCommits = await fetch(apiUrlAllCommits);
    const commitsData: TypeApiUrlAllCommits[] = await fetchAllCommits.json();
    const allShaCommitCodes = commitsData.map(
      (thisCommit: TypeApiUrlAllCommits) => thisCommit.sha,
    );
    const allRawUrl = allShaCommitCodes.map(
      (thisShaCode) =>
        `https://raw.githubusercontent.com/${owner}/${repoName}/${thisShaCode}/${filePath}`,
    );
    const allRawCodeFileStrings = await Promise.all(
      allRawUrl.map(async (thisRawUrl) => {
        const fetchRawFile = await fetch(thisRawUrl);
        return await fetchRawFile.text();
      }),
    );
    return allRawCodeFileStrings;
  };
</script>
