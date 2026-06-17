$raw = Get-Content 'C:\Users\Admin\.gemini\antigravity-ide\brain\2ae73029-35e8-4598-aa5a-b75892b90742\.system_generated\steps\12\content.md' -Raw
$parts = $raw -split '---'
$jsonData = $parts[1].Trim()
$repos = $jsonData | ConvertFrom-Json

$languages = @{}
$originalCount = 0
$forkCount = 0
$totalStars = 0

foreach ($r in $repos) {
    Write-Output "$($r.name) | fork:$($r.fork) | lang:$($r.language) | stars:$($r.stargazers_count) | forks:$($r.forks_count) | created:$($r.created_at)"
    $totalStars += $r.stargazers_count
    if ($r.fork) { $forkCount++ } else { $originalCount++ }
    if ($r.language) {
        if ($languages.ContainsKey($r.language)) {
            $languages[$r.language]++
        } else {
            $languages[$r.language] = 1
        }
    }
}

Write-Output ""
Write-Output "=== SUMMARY ==="
Write-Output "Total repos: $($repos.Count)"
Write-Output "Original: $originalCount | Forked: $forkCount"
Write-Output "Total stars: $totalStars"
Write-Output ""
Write-Output "Languages:"
foreach ($lang in $languages.GetEnumerator() | Sort-Object Value -Descending) {
    Write-Output "  $($lang.Key): $($lang.Value) repos"
}
