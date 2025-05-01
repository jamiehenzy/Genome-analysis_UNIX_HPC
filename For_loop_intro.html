<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Unix for Biologists: For Loop Tutorial</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 800px;
      margin: auto;
      line-height: 1.6;
      padding: 20px;
    }
    code {
      background-color: #f4f4f4;
      padding: 2px 6px;
      border-radius: 4px;
    }
    pre {
      background-color: #f9f9f9;
      padding: 10px;
      border-left: 4px solid #ccc;
      overflow-x: auto;
    }
    .exercise {
      background: #e0f7fa;
      padding: 10px;
      border-left: 5px solid #00796b;
      margin: 20px 0;
    }
    .feedback {
      background: #fff3e0;
      padding: 10px;
      border-left: 5px solid #f57c00;
    }
  </style>
</head>
<body>
  <h1>Unix for Biologists: For Loop Tutorial</h1>

  <p><strong>Learning goals:</strong> Use <code>for</code> loops to process multiple files, extract base names using <code>basename</code> and shell parameter expansion, and simulate file-based workflows.</p>

  <h2>Setup</h2>
  <pre><code>mkdir -p ~/unix_tutorial/seq_data
cd ~/unix_tutorial/seq_data
touch SRR00{1..6}.fastq</code></pre>

  <h2>Part 1: Basic For Loop</h2>
  <pre><code>for file in *.fastq; do
  echo "Processing $file"
done</code></pre>

  <div class="feedback">
    <strong>Expected output:</strong> One line per file, like <code>Processing SRR001.fastq</code>
  </div>

  <h2>Part 2: Extract base name with basename</h2>
  <pre><code>for file in *.fastq; do
  base=$(basename "$file" .fastq)
  echo "Base name: $base"
done</code></pre>

  <h2>Part 3: Extract base name with shell expansion</h2>
  <pre><code>for file in *.fastq; do
  base="${file%.fastq}"
  echo "Base name: $base"
done</code></pre>

  <div class="exercise">
    <strong>Exercise 1:</strong> Try this with <code>time</code> and compare speed:
    <pre><code>time for file in *.fastq; do base=$(basename "$file" .fastq); done
time for file in *.fastq; do base="${file%.fastq}"; done</code></pre>
  </div>

  <h2>Part 4: Create directories per sample</h2>
  <pre><code>for file in *.fastq; do
  sample="${file%.fastq}"
  mkdir "$sample"
  echo "Created folder for $sample"
done</code></pre>

  <h2>Part 5: Simulate output file per sample</h2>
  <pre><code>for file in *.fastq; do
  sample="${file%.fastq}"
  echo "Simulated analysis of $file" > "$sample/${sample}_log.txt"
done</code></pre>

  <div class="exercise">
    <strong>Exercise 2:</strong> Check that each folder has a <code>_log.txt</code> file inside.
  </div>

  <h2>Challenge: Rename .fastq to .fq</h2>
  <pre><code>for file in *.fastq; do
  new="${file%.fastq}.fq"
  mv "$file" "$new"
done</code></pre>

  <h2>Wrap-Up Quiz</h2>
  <pre><code>for file in *.fq; do
  echo "Now working on: ${file%.fq}"
done</code></pre>

  <div class="feedback">
    <strong>Expected:</strong> For <code>SRR004.fq</code>, it should print <code>Now working on: SRR004</code>
  </div>
</body>
</html>
