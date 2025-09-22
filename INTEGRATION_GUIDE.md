# Ephemeral Test Suite Integration Guide

This guide explains how to integrate the Ephemeral Test Suite into your main repository using Method 4 (Composite Action).

## 🎯 Overview

The Ephemeral Test Suite provides a comprehensive testing solution with:
- **Unit Tests**: Fast, isolated component tests
- **Integration Tests**: PostgreSQL database testing
- **Selenium Tests**: Browser automation and UI testing

All tests run in Docker containers for consistent, isolated environments.

## 📁 Files in this Directory

### GitHub Actions Workflows
- `test.yml` - Basic test workflow for CI/CD
- `test-matrix.yml` - Parallel test execution
- `ci-cd.yml` - Complete CI/CD pipeline

### Setup Scripts
- `setup-test-suite.sh` - Bash setup script (Linux/macOS)
- `setup-test-suite.ps1` - PowerShell setup script (Windows)
- `update-test-suite.sh` - Update script for newer versions

### Configuration
- `test-config.yml` - Test suite configuration
- `README.md` - Integration documentation

## 🚀 Quick Start

### Option 1: Automated Setup (Recommended)

#### For Linux/macOS:
```bash
# Copy files to your main repository
cp -r main/* /path/to/your/main/repo/

# Run setup script
cd /path/to/your/main/repo
chmod +x setup-test-suite.sh
./setup-test-suite.sh
```

#### For Windows:
```powershell
# Copy files to your main repository
Copy-Item -Recurse main\* C:\path\to\your\main\repo\

# Run setup script
cd C:\path\to\your\main\repo
.\setup-test-suite.ps1
```

### Option 2: Manual Setup

1. **Copy workflow files** to `.github/workflows/` in your main repository
2. **Update repository references** in all workflow files:
   - Replace `yourusername/ephemeral_poc` with your test repository
   - Replace `your-main-repo` with your main repository name
3. **Copy configuration files** to your main repository root
4. **Update `.gitignore`** to exclude test results

## 🔧 Configuration

### Test Repository Reference

Update the test repository reference in your workflows:

```yaml
# In your workflow files
uses: yourusername/ephemeral_poc@main
```

Or use environment variables:

```yaml
env:
  TEST_REPO: yourusername/ephemeral_poc
  TEST_BRANCH: main

# Then use:
uses: ${{ env.TEST_REPO }}@${{ env.TEST_BRANCH }}
```

### Test Configuration

Edit `test-config.yml` to customize:

```yaml
test_suite:
  repository: yourusername/ephemeral_poc
  branch: main

defaults:
  timeout: 600
  upload_results: true

timeouts:
  unit: 300
  integration: 600
  selenium: 600
  all: 900
```

## 📊 Workflow Types

### 1. Basic Test Workflow (`test.yml`)
- Triggers on push/PR to main branches
- Supports manual triggering
- Single test execution

### 2. Test Matrix Workflow (`test-matrix.yml`)
- Runs all test types in parallel
- Provides detailed summaries
- Optimized for speed

### 3. CI/CD Pipeline (`ci-cd.yml`)
- Staged testing approach
- Unit tests → Integration tests → Selenium tests
- Full test suite on main branch
- Build and deploy steps

### 4. Manual Test Workflow (`manual-test.yml`)
- Manual triggering only
- Customizable parameters
- Detailed result summaries

## 🛠️ Customization

### Adding Custom Test Types

1. **Update the composite action** in your test repository
2. **Add new test types** to the workflow inputs
3. **Update configuration** files

### Environment Variables

Add environment variables to your workflows:

```yaml
- name: Run Tests
  uses: yourusername/ephemeral_poc@main
  with:
    test-type: 'all'
  env:
    CUSTOM_VAR: 'value'
    POSTGRES_HOST: 'postgres'
```

### Custom Timeouts

Set different timeouts for different test types:

```yaml
- name: Run Unit Tests
  uses: yourusername/ephemeral_poc@main
  with:
    test-type: 'unit'
    timeout: '300'
```

## 🔄 Updating the Test Suite

### Using the Update Script

```bash
# Update to a new branch
./update-test-suite.sh --branch develop

# Update to a specific tag
./update-test-suite.sh --tag v1.2.0

# Update to a different repository
./update-test-suite.sh --repo myorg/ephemeral_poc --branch main
```

### Manual Update

1. **Update workflow files** with new repository references
2. **Update configuration** files
3. **Test the changes** with a pull request

## 📈 Monitoring and Results

### Test Results

Test results are automatically uploaded as artifacts:
- `ephemeral-test-results-unit`
- `ephemeral-test-results-integration`
- `ephemeral-test-results-selenium`
- `ephemeral-test-results-all`

### GitHub Actions Summary

The workflows generate detailed summaries showing:
- Test execution status
- Execution logs
- Performance metrics
- Error details

### Notifications

Configure notifications in your workflows:

```yaml
- name: Notify on Failure
  if: failure()
  uses: 8398a7/action-slack@v3
  with:
    status: failure
    webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

## 🐛 Troubleshooting

### Common Issues

1. **Tests Timeout**
   - Increase timeout values in workflows
   - Check Docker resource limits

2. **Docker Issues**
   - Ensure Docker is available in GitHub Actions
   - Check container health status

3. **Test Failures**
   - Review uploaded artifacts for detailed logs
   - Check environment variables

4. **Repository Not Found**
   - Verify repository references are correct
   - Check repository permissions

### Debug Mode

Enable debug logging:

```yaml
- name: Run Tests
  uses: yourusername/ephemeral_poc@main
  with:
    test-type: 'all'
  env:
    ACTIONS_STEP_DEBUG: true
```

### Local Testing

Test locally before pushing:

```bash
# Clone the test repository
git clone https://github.com/yourusername/ephemeral_poc.git
cd ephemeral_poc

# Run tests
python test_runner.py all
```

## 📚 Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Docker Documentation](https://docs.docker.com/)
- [Ephemeral Test Suite Repository](https://github.com/yourusername/ephemeral_poc)

## 🤝 Support

For issues with the test suite integration:

1. Check the [troubleshooting section](#-troubleshooting)
2. Review the test repository documentation
3. Check GitHub Actions logs for detailed error messages
4. Create an issue in the test repository

## 📄 License

This integration guide is provided as-is. See the test repository for license information.
