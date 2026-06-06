# automated_security_report.py

**Automated Security Assessment Report Generator**

A production-grade, SANS-aligned automated reporting tool written in Python that dynamically compiles infrastructure security vulnerability data into professional, executive-ready assessment reports with minimal configuration.

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Usage Examples](#usage-examples)
- [Configuration](#configuration)
- [Output Examples](#output-examples)
- [API Reference](#api-reference)
- [Requirements](#requirements)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

This tool automates the final delivery phase of a security assessment or automated vulnerability scan. It parses complex scanning data (JSON/dictionaries) and structures it into a professionally formatted report that meets SANS assessment standards. Perfect for security teams, penetration testers, and vulnerability management professionals who need to generate consistent, branded assessment reports efficiently.

### Why Use This Tool?

- **Time Savings**: Reduce report generation time from hours to minutes
- **Consistency**: Maintain SANS-aligned structure across all assessments
- **Professionalism**: Generate executive-ready reports automatically
- **Flexibility**: Works with any vulnerability scanning tool's JSON output
- **Cloud Native**: Built with Google Colab environments in mind

---

## Key Features

- **SANS-Aligned Structure**: Seamlessly transitions from an Executive Summary down into high-level prioritization grids and precise technical remediation tasks.
- **Dynamic Data Visuals**: Automatically draws structured finding grids adjusting text wrapping dynamically with custom data matrix spacing.
- **Automated Risk Grading**: Features conditional layout processing to color-code criticalities—instantly drawing eye-catching highlights for High, Moderate, and Low findings.
- **Cloud Notebook Native**: Out-of-the-box support for Google Colab environments including pip auto-provisioning and native browser file downloads.
- **Customizable Templates**: Brand your reports with custom headers, footers, and color schemes
- **Multi-Format Support**: Generate reports in HTML, PDF, and Markdown formats
- **Batch Processing**: Process multiple vulnerability scans in one execution

---

## Installation

### Prerequisites

- Python 3.8 or higher
- pip package manager

### From PyPI

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

---

## Quick Start

### Basic Usage

```python
from automated_security_report import SecurityReportGenerator

# Load vulnerability data
vulnerability_data = {
    "scan_date": "2024-01-15",
    "target": "example.com",
    "findings": [
        {
            "id": "VULN-001",
            "title": "SQL Injection",
            "severity": "High",
            "cvss_score": 9.2,
            "description": "SQL injection vulnerability in login form",
            "remediation": "Use parameterized queries"
        }
    ]
}

# Generate report
generator = SecurityReportGenerator(data=vulnerability_data)
generator.generate_report(
    output_format="html",
    output_file="security_assessment_2024.html"
)
```

### In Google Colab

```python
# Install
!pip install automated-security-report

# Import and use
from automated_security_report import SecurityReportGenerator
from google.colab import files

# Load data
uploaded = files.upload()

# Generate and download
generator = SecurityReportGenerator(data=your_data)
generator.generate_report("report.html")
files.download("report.html")
```

---

## Usage Examples

### Example 1: Parse Nessus JSON Export

```python
import json
from automated_security_report import SecurityReportGenerator

# Load Nessus export
with open("nessus_export.json") as f:
    nessus_data = json.load(f)

# Convert to standard format
vulnerability_data = convert_nessus_format(nessus_data)

# Generate report
generator = SecurityReportGenerator(data=vulnerability_data)
generator.generate_report("nessus_assessment.html")
```

### Example 2: Custom Configuration

```python
from automated_security_report import SecurityReportGenerator, ReportConfig

config = ReportConfig(
    title="2024 Q1 Security Assessment",
    organization="Acme Corp",
    theme="dark",
    include_executive_summary=True,
    include_remediation_timeline=True,
    color_scheme="custom",
    high_color="#ff6b6b",
    medium_color="#ffd93d",
    low_color="#6bcf7f"
)

generator = SecurityReportGenerator(data=vulnerability_data, config=config)
generator.generate_report("custom_report.html")
```

### Example 3: Batch Processing

```python
from automated_security_report import batch_generate_reports

scan_files = [
    "scan_app1.json",
    "scan_app2.json",
    "scan_infrastructure.json"
]

batch_generate_reports(
    input_files=scan_files,
    output_directory="./reports",
    format="pdf"
)
```

---

## Configuration

### ReportConfig Options

```python
ReportConfig(
    # Report metadata
    title="Security Assessment Report",
    organization="Your Organization",
    assessment_date="2024-01-15",
    assessor_name="Security Team",
    
    # Formatting
    theme="light",  # "light" or "dark"
    color_scheme="default",  # "default", "professional", "custom"
    high_color="#ff0000",
    medium_color="#ffaa00",
    low_color="#00cc00",
    
    # Content options
    include_executive_summary=True,
    include_remediation_timeline=True,
    include_risk_matrix=True,
    include_technical_details=True,
    
    # Output
    page_size="letter",  # "letter" or "a4"
    orientation="portrait",
    
    # Advanced
    custom_header_image="path/to/logo.png",
    custom_footer_text="Confidential - Internal Use Only"
)
```

---

## Output Examples

### Generated Report Sections

1. **Cover Page** - Organization branding and assessment metadata
2. **Executive Summary** - High-level findings overview and risk metrics
3. **Risk Matrix** - Visual representation of finding distribution
4. **Findings Details** - Comprehensive vulnerability information:
   - Vulnerability ID
   - Title and Description
   - CVSS Score and Severity
   - Affected Systems
   - Business Impact
   - Remediation Steps
   - Timeline Recommendations

5. **Appendices** - Technical details, tool versions, methodology

---

## API Reference

### SecurityReportGenerator

```python
class SecurityReportGenerator:
    def __init__(self, data: dict, config: ReportConfig = None):
        """Initialize report generator with vulnerability data."""
        
    def generate_report(self, 
                       output_format: str = "html",
                       output_file: str = "report.html") -> None:
        """Generate formatted security report."""
        
    def add_custom_section(self, title: str, content: str) -> None:
        """Add custom section to report."""
        
    def set_theme(self, theme: str) -> None:
        """Update report theme dynamically."""
```

### Supported Data Format

```python
{
    "scan_date": "2024-01-15",
    "target": "example.com",
    "assessor": "Security Team",
    "findings": [
        {
            "id": "VULN-001",
            "title": "Vulnerability Title",
            "severity": "High|Medium|Low",
            "cvss_score": 9.2,
            "description": "Detailed description",
            "affected_systems": ["system1", "system2"],
            "remediation": "Steps to fix",
            "timeline": "30 days"
        }
    ]
}
```

---

## Requirements

### Core Dependencies

- Python 3.8+
- jinja2 >= 3.0
- reportlab >= 4.0
- pydantic >= 2.0

### Optional Dependencies

```bash
# For PDF generation
pip install automated-security-report[pdf]

# For all extras
pip install automated-security-report[all]
```

### Full requirements.txt

```
jinja2>=3.0
reportlab>=4.0
pydantic>=2.0
python-dotenv>=0.19.0
requests>=2.28.0
```

---

## Contributing

We welcome contributions! Here's how to get involved:

### Reporting Issues

- Check existing issues first
- Provide clear description and reproduction steps
- Include your Python version and OS

### Submitting Pull Requests

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Setup

```bash
git clone https://github.com/bellayacoback-oss/automated_security_report.py.git
cd automated_security_report.py
pip install -r requirements-dev.txt
pytest
```

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Support & Contact

- **Documentation**: [Full docs](https://github.com/bellayacoback-oss/automated_security_report.py/wiki)
- **Issues**: [GitHub Issues](https://github.com/bellayacoback-oss/automated_security_report.py/issues)
- **Discussions**: [GitHub Discussions](https://github.com/bellayacoback-oss/automated_security_report.py/discussions)

---

**Last Updated**: January 2024 | **Version**: 1.0.0
