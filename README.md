# GitHub Repository Security Analysis

A Python-based research tool for analyzing security vulnerabilities in popular GitHub repositories from 2017-2025. This project collects repositories based on star count and contributor thresholds, then performs comprehensive security scanning using Semgrep and dependency vulnerability detection.

## Features

- **Repository Collection**: Fetches GitHub repositories meeting configurable criteria (minimum stars, contributors, languages)
- **CVE Detection**: Uses Semgrep to scan for security vulnerabilities and code patterns matching CWE/OWASP categories
- **Dependency Scanning**: Identifies insecure dependencies using pip-audit and npm audit
- **Data Visualization**: Generates comprehensive graphs showing vulnerability distribution, trends over time, and language comparisons
- **Data Persistence**: Caches intermediate results to allow resuming long-running analyses

## Requirements

- Python 3.8+
- Git
- Semgrep CLI
- Node.js/npm (for JavaScript dependency scanning)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/AAR-Research-Paper-AI-Effects.git
cd AAR-Research-Paper-AI-Effects
```

2. Install Python dependencies:
```bash
pip install -r requirements.txt
```

3. Install Semgrep:
```bash
pip install semgrep
# or
brew install semgrep  # macOS
```

4. Set up GitHub API credentials:
```bash
cp keys.env.example keys.env
# Edit keys.env and add your GitHub Personal Access Token
```

### Creating a GitHub Token

1. Go to [GitHub Settings > Developer Settings > Personal Access Tokens](https://github.com/settings/tokens)
2. Click "Generate new token (classic)"
3. Select scopes: `public_repo` (for reading public repositories)
4. Copy the token and paste it into `keys.env`

## Usage

1. Open the Jupyter notebook:
```bash
jupyter notebook github_security_analysis.ipynb
```

2. Configure the analysis parameters in the CONFIG cell:
```python
CONFIG = {
    'min_stars': 500,              # Minimum stars for repository
    'min_contributors': 10,        # Minimum contributors
    'start_year': 2017,            # Analysis start year
    'end_year': 2025,              # Analysis end year
    'languages': ['Python', 'JavaScript', 'TypeScript', 'Java', 'Go'],
    'max_repos_per_language': 100, # Max repos to fetch per language
}
```

3. Run all cells to execute the analysis

## Output

The analysis generates the following outputs:

### Data Files (in `results/` directory)
- `security_analysis_report.xlsx` - Complete Excel report with all data
- `semgrep_findings.csv` - All Semgrep vulnerability findings
- `dependency_vulnerabilities.csv` - All dependency vulnerabilities
- `repositories.csv` - Repository metadata
- `summary.csv` - Summary statistics

### Visualizations (in `results/figures/` directory)
- `severity_distribution.png` - Distribution of findings by severity
- `language_distribution.png` - Vulnerabilities by programming language
- `temporal_trends.png` - Vulnerability trends from 2017-2025
- `cwe_distribution.png` - Top CWE categories found
- `dependency_analysis.png` - Dependency vulnerability analysis
- `heatmap_interactive.html` - Interactive language vs year heatmap
- `sunburst_interactive.html` - Hierarchical vulnerability breakdown
- `scatter_interactive.html` - Stars vs vulnerability count scatter plot

### Cached Data (in `data/` directory)
- `repositories_cache.joblib` - Cached repository metadata
- `semgrep_results.joblib` - Cached Semgrep scan results
- `dependency_results.joblib` - Cached dependency scan results

## Project Structure

```
AAR-Research-Paper-AI-Effects/
├── github_security_analysis.ipynb  # Main analysis notebook
├── requirements.txt                 # Python dependencies
├── keys.env.example                 # API key template
├── keys.env                         # Your API keys (gitignored)
├── data/                            # Cached data (gitignored)
├── repos/                           # Cloned repositories (gitignored)
├── results/                         # Output files (gitignored)
│   └── figures/                     # Generated visualizations
├── .gitignore
├── LICENSE
└── README.md
```

## Analysis Components

### 1. Repository Collection
Queries the GitHub API to find repositories matching criteria:
- Minimum star count (default: 500)
- Minimum contributors (default: 10)
- Created between 2017-2025
- Specific programming languages

### 2. Semgrep Security Scanning
Scans cloned repositories for:
- OWASP Top 10 vulnerabilities
- CWE Top 25 weaknesses
- Language-specific security issues
- Code injection patterns
- Authentication/authorization flaws

### 3. Dependency Vulnerability Scanning
Checks package manifests for known vulnerabilities:
- Python: `requirements.txt`, `setup.py`, `pyproject.toml`
- JavaScript/Node: `package.json`, `package-lock.json`
- Supports pip-audit and npm audit

### 4. Data Visualization
Generates both static (matplotlib/seaborn) and interactive (plotly) visualizations showing:
- Vulnerability severity distribution
- Language-wise vulnerability density
- Temporal trends (2017-2025)
- CWE category breakdown
- Dependency ecosystem analysis
- Repository characteristics vs vulnerability correlation

## Rate Limiting

The GitHub API has rate limits (5000 requests/hour for authenticated users). The tool:
- Automatically detects rate limit hits
- Waits for rate limit reset
- Caches results to minimize API calls
- Supports multiple tokens for higher throughput

## Notes

- Cloned repositories can consume significant disk space
- Semgrep scans may be slow for large repositories
- Results are cached in `data/` for resumable analysis
- Set `use_cache=False` to force re-scanning

## License

MIT License - see [LICENSE](LICENSE) for details.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.
