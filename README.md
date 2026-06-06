# automated_security_report.py

[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

**Automated Security Assessment Report Generator**

A production-grade, SANS-aligned automated reporting tool written in Python that dynamically compiles infrastructure security vulnerability data into professional, executive-ready assessment reports. Perfect for security professionals, penetration testers, and DevSecOps teams.

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Usage Examples](#usage-examples)
- [Configuration](#configuration)
- [Output Examples](#output-examples)
- [API Reference](#api-reference)
- [Troubleshooting](#troubleshooting)
- [Performance](#performance)
- [Requirements](#requirements)
- [Contributing](#contributing)
- [Security](#security)
- [License](#license)

---

## Overview

This tool automates the final delivery phase of a security assessment or vulnerability scan. It parses complex scanning data (JSON/dictionaries) and structures it into professionally formatted, executive-ready reports aligned with industry standards (SANS, NIST). 

**Why spend 8 hours formatting a report when this can do it in 8 seconds?**

### Why Use This Tool?

- **⏱️ Time Savings**: Reduce report generation from hours to minutes
- **✅ Consistency**: Maintain SANS-aligned structure across all assessments
- **🎯 Professionalism**: Generate executive-ready reports automatically
- **🔄 Flexibility**: Works with any vulnerability scanning tool's JSON output
- **☁️ Cloud Native**: Built for Google Colab with native browser downloads
- **🎨 Customizable**: Brand reports with your organization's colors and logos

---

## Key Features

- **SANS-Aligned Structure**: Seamlessly transitions from Executive Summary through risk grids to technical remediation tasks
- **Dynamic Data Visuals**: Automatically draws structured finding grids with adaptive text wrapping and custom spacing
- **Automated Risk Grading**: Color-codes findings by severity (High, Medium, Low) with eye-catching visual emphasis
- **Cloud Notebook Native**: Out-of-the-box support for Google Colab including auto-provisioning and downloads
- **Customizable Templates**: Brand with custom headers, footers, logos, and color schemes
- **Multi-Format Support**: Generate HTML, PDF, and Markdown formats
- **Batch Processing**: Process multiple vulnerability scans in a single execution
- **Built-in Parsers**: Pre-configured parsers for Nessus, OpenVAS, and custom formats

---

## Quick Start

### 30-Second Example

```python
from automated_security_report import SecurityReportGenerator

# Define your findings
findings = {
    "scan_date": "2024-06-06",
    "target": "example.com",
    "findings": [
        {
            "id": "VULN-001",
            "title": "SQL Injection",
            "severity": "High",
            "cvss_score": 9.2,
            "description": "SQL injection in login form",
            "remediation": "Use parameterized queries",
            "affected_systems": ["app-server-01"]
        }
    ]
}

# Generate report
generator = SecurityReportGenerator(data=findings)
generator.generate_report(output_format="html", output_file="report.html")
print("✓ Report generated: report.html")
```

---

## Installation

### Prerequisites

- Python 3.9 or higher
- pip package manager

### From PyPI (Recommended)

```bash
pip install automated-security-report
```

### From Source

```bash
git clone https://github.com/bellayacoback-oss/automated_security_report.py.git
cd automated_security_report.py
pip install -r requirements.txt
```

### For Google Colab

```python
!pip install automated-security-report
```

### Verify Installation

```python
import automated_security_report
print(automated_security_report.__version__)  # Verify successful import
```

---

## Usage Examples

### Example 1: Parse Nessus JSON Export

```python
import json
from automated_security_report import SecurityReportGenerator, parsers

# Load Nessus export
with open("nessus_export.json") as f:
    nessus_data = json.load(f)

# Parse and convert
vulnerability_data = parsers.parse_nessus(nessus_data)

# Generate report
generator = SecurityReportGenerator(data=vulnerability_data)
generator.generate_report(
    output_format="html",
    output_file="nessus_assessment.html"
)
```

### Example 2: Custom Configuration with Branding

```python
from automated_security_report import SecurityReportGenerator, ReportConfig

config = ReportConfig(
    # Organization details
    title="2024 Q2 Security Assessment",
    organization="Acme Corp",
    assessor_name="Security Team",
    assessment_date="2024-06-06",
    
    # Branding
    theme="dark",
    color_scheme="custom",
    high_color="#ff6b6b",
    medium_color="#ffd93d",
    low_color="#6bcf7f",
    custom_header_image="logo.png",
    custom_footer_text="Confidential - Internal Use Only",
    
    # Content
    include_executive_summary=True,
    include_remediation_timeline=True,
    include_risk_matrix=True,
    include_technical_details=True
)

generator = SecurityReportGenerator(data=vulnerability_data, config=config)
generator.generate_report(output_format="pdf", output_file="custom_report.pdf")
```

### Example 3: Batch Processing Multiple Scans

```python
from automated_security_report import batch_generate_reports
import glob

# Process all scan files
scan_files = glob.glob("scans/*.json")

batch_generate_reports(
    input_files=scan_files,
    output_directory="./reports",
    format="html",
    skip_errors=False  # Stop on first error
)

print(f"✓ Generated {len(scan_files)} reports")
```

### Example 4: Google Colab Integration

```python
# Install
!pip install -q automated-security-report

# Import and upload
from automated_security_report import SecurityReportGenerator
from google.colab import files
import json

# Upload vulnerability data
print("Upload your vulnerability scan JSON file:")
uploaded = files.upload()

# Parse uploaded file
filename = list(uploaded.keys())[0]
with open(filename) as f:
    scan_data = json.load(f)

# Generate report
generator = SecurityReportGenerator(data=scan_data)
generator.generate_report(output_format="html", output_file="report.html")

# Download
print("✓ Report ready!")
files.download("report.html")
```

### Example 5: Error Handling

```python
from automated_security_report import SecurityReportGenerator, ValidationError
import logging

logging.basicConfig(level=logging.INFO)

try:
    generator = SecurityReportGenerator(data=vulnerability_data)
    generator.generate_report(output_file="report.html")
except ValidationError as e:
    logging.error(f"Data validation failed: {e}")
except FileNotFoundError as e:
    logging.error(f"Output directory not found: {e}")
except Exception as e:
    logging.error(f"Unexpected error: {e}")
```

---

## Configuration

### ReportConfig Options

```python
ReportConfig(
    # Report metadata
    title="Security Assessment Report",
    organization="Your Organization",
    assessment_date="2024-06-06",
    assessor_name="Security Team",
    
    # Formatting & Branding
    theme="light",                          # "light" or "dark"
    color_scheme="default",                 # "default", "professional", "vibrant", "custom"
    high_color="#ff0000",                   # Hex color for High severity
    medium_color="#ffaa00",                 # Hex color for Medium severity
    low_color="#00cc00",                    # Hex color for Low severity
    custom_header_image="path/to/logo.png", # Organization logo
    custom_footer_text="Company Confidential",
    
    # Content options
    include_executive_summary=True,
    include_remediation_timeline=True,
    include_risk_matrix=True,
    include_technical_details=True,
    include_methodology=True,
    
    # Output formatting
    page_size="letter",                     # "letter" or "a4"
    orientation="portrait",                 # "portrait" or "landscape"
    
    # Advanced
    compress_pdf=False,                     # For PDF format
    metadata_author="Security Team",
    enable_toc=True                         # Table of contents
)
```

---

## Output Examples

### Generated Report Sections

1. **Cover Page** - Organization branding, assessment date, metadata
2. **Executive Summary** - Key findings, risk metrics, top recommendations
3. **Risk Matrix** - Visual grid of finding distribution by severity
4. **Detailed Findings** - For each vulnerability:
   - Vulnerability ID & Title
   - CVSS Score & Severity Rating
   - Description & Technical Impact
   - Affected Systems & Business Context
   - Step-by-Step Remediation
   - Timeline Recommendations
5. **Appendices** - Tools used, methodology, assessment scope

### Sample Output
```
[REPORT GENERATED]
Format: HTML
File Size: 2.3 MB
Findings: 47 total
  - High: 8
  - Medium: 19
  - Low: 20
Generation Time: 3.2 seconds
```

---

## API Reference

### SecurityReportGenerator

```python
class SecurityReportGenerator:
    def __init__(self, data: dict, config: ReportConfig = None):
        """
        Initialize report generator with vulnerability data.
        
        Args:
            data: Vulnerability scan dictionary
            config: Optional ReportConfig for customization
            
        Raises:
            ValidationError: If data format is invalid
        """
        
    def generate_report(self, 
                       output_format: str = "html",
                       output_file: str = "report.html") -> str:
        """
        Generate formatted security report.
        
        Args:
            output_format: "html", "pdf", or "markdown"
            output_file: Output file path
            
        Returns:
            Path to generated report
            
        Raises:
            FileNotFoundError: If output directory doesn't exist
            ValueError: If unsupported format requested
        """
        
    def add_custom_section(self, title: str, content: str) -> None:
        """Add custom section to report."""
        
    def set_theme(self, theme: str) -> None:
        """Update report theme dynamically."""
        
    def validate_data(self) -> bool:
        """Validate input data against schema."""
```

### Supported Input Format

```python
{
    "scan_date": "2024-06-06",              # ISO format date
    "target": "example.com",                # Target of assessment
    "assessor": "Security Team",            # Your team name
    "organization": "Acme Corp",            # Optional
    "findings": [
        {
            "id": "VULN-001",               # Unique identifier
            "title": "Vulnerability Title",
            "severity": "High",             # High, Medium, Low
            "cvss_score": 9.2,              # 0.0 - 10.0
            "description": "Detailed technical description",
            "affected_systems": ["web-01", "app-02"],
            "business_impact": "Data breach risk",
            "remediation": "Step-by-step fix instructions",
            "timeline": "30 days",
            "cve": "CVE-2024-12345"         # Optional CVE reference
        }
    ]
}
```

---

## Troubleshooting

### Common Issues

#### "ModuleNotFoundError: No module named 'automated_security_report'"

```bash
# Solution: Install the package
pip install --upgrade automated-security-report
```

#### "ValidationError: Invalid severity level"

**Issue**: Severity must be "High", "Medium", or "Low"

```python
# ❌ Wrong
"severity": "CRITICAL"

# ✅ Correct
"severity": "High"
```

#### "FileNotFoundError: Output directory does not exist"

```python
# Solution: Create directory first
import os
os.makedirs("./reports", exist_ok=True)
```

#### PDF generation is slow

```python
# Enable compression for faster PDF generation
config = ReportConfig(compress_pdf=True)
```

#### Large scans (1000+ findings) run out of memory

```python
# Use batch processing instead
from automated_security_report import batch_generate_reports

# Split into smaller files and process
batch_generate_reports(input_files=scan_files, output_directory="./reports")
```

---

## Performance

| Metric | Value |
|--------|-------|
| Small report (1-10 findings) | ~0.5s |
| Medium report (10-100 findings) | ~2-3s |
| Large report (100-500 findings) | ~10-15s |
| HTML generation | ~0.5s faster than PDF |
| PDF with compression | 30-50% smaller file size |

**Memory Usage**: ~50MB for reports with <500 findings

---

## Requirements

### Core Dependencies

- Python 3.9+
- jinja2 >= 3.1
- reportlab >= 4.0
- pydantic >= 2.0

### Optional Dependencies

```bash
# For PDF generation with advanced features
pip install automated-security-report[pdf]

# For all extras (includes development tools)
pip install automated-security-report[all]

# For development/testing
pip install automated-security-report[dev]
```

### Full requirements.txt

```
jinja2>=3.1
reportlab>=4.0
pydantic>=2.0
python-dotenv>=0.19.0
requests>=2.28.0
Pillow>=9.0
```

---

## Contributing

We welcome contributions! Here's how:

### Reporting Issues

- Search [existing issues](https://github.com/bellayacoback-oss/automated_security_report.py/issues) first
- Include: Python version, OS, minimal reproduction code
- Use issue templates provided

### Submitting Pull Requests

1. Fork the repository
2. Create feature branch: `git checkout -b feature/your-feature`
3. Make changes and test: `pytest tests/`
4. Format code: `black .`
5. Commit: `git commit -m "Add feature: description"`
6. Push: `git push origin feature/your-feature`
7. Open Pull Request with description

### Development Setup

```bash
git clone https://github.com/bellayacoback-oss/automated_security_report.py.git
cd automated_security_report.py
pip install -r requirements-dev.txt
pytest          # Run tests
black .         # Format code
pylint src/     # Lint check
```

---

## Security

### Reporting Security Issues

**Do not open public issues for security vulnerabilities.**

Email: security@example.com (replace with actual contact)

Please include:
- Description of vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (optional)

### Security Considerations

- Reports contain sensitive data; store securely
- Use HTTPS for cloud uploads
- Sanitize custom headers/footers to prevent XSS
- PDF files are not encrypted by default

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Support & Contact

- **📖 Documentation**: [Full wiki](https://github.com/bellayacoback-oss/automated_security_report.py/wiki)
- **🐛 Issues**: [GitHub Issues](https://github.com/bellayacoback-oss/automated_security_report.py/issues)
- **💬 Discussions**: [GitHub Discussions](https://github.com/bellayacoback-oss/automated_security_report.py/discussions)
- **📧 Email**: support@example.com

---

**Version**: 1.2.0 | **Last Updated**: June 2026 | **Status**: Production-Ready ✓
