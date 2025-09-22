# Main Repository with Ephemeral Test Suite

This repository uses the [Ephemeral POC Test Suite](https://github.com/yourusername/ephemeral_poc) as a composite GitHub Action for comprehensive testing.

## 🧪 Test Suite Integration

This project integrates a comprehensive test suite that includes:

- **Unit Tests**: Fast, isolated tests for individual components
- **Integration Tests**: PostgreSQL database connectivity and operations
- **Selenium Tests**: Browser automation and UI testing

All tests run in Docker containers for consistent, isolated environments.

## 🚀 GitHub Actions Workflows

### 1. Basic Test Workflow (`.github/workflows/test.yml`)
- Runs on push to main/develop branches
- Runs on pull requests
- Supports manual triggering with test type selection
- Uses the ephemeral test suite composite action

### 2. Test Matrix Workflow (`.github/workflows/test-matrix.yml`)
- Runs all test types in parallel
- Provides detailed test results summary
- Optimized timeouts for each test type

### 3. CI/CD Pipeline (`.github/workflows/ci-cd.yml`)
- Staged testing approach:
  1. Unit tests (fast feedback)
  2. Integration tests (database functionality)
  3. Selenium tests (UI functionality)
  4. Full test suite (main branch only)
- Build and deploy steps

## 🔧 Configuration

### Test Repository Reference
Update the test repository reference in your workflows:

```yaml
# In your workflow files, update these values:
env:
  TEST_REPO: yourusername/ephemeral_poc  # Your test repo
  TEST_BRANCH: main                      # Branch to use
```

### Customizing Test Execution

The composite action supports several inputs:

```yaml
- name: Run Tests
  uses: yourusername/ephemeral_poc@main
  with:
    test-type: 'all'           # unit, integration, selenium, or all
    timeout: '600'             # Timeout in seconds
    working-directory: '.'     # Working directory
    upload-results: 'true'     # Upload test results as artifacts
```

## 📊 Test Results

Test results are automatically uploaded as GitHub Actions artifacts:
- **Unit Test Results**: `ephemeral-test-results-unit`
- **Integration Test Results**: `ephemeral-test-results-integration`
- **Selenium Test Results**: `ephemeral-test-results-selenium`
- **All Tests Results**: `ephemeral-test-results-all`

Results are stored in JUnit XML format for CI/CD integration and are retained for 7 days.

## 🛠️ Local Development

To run tests locally, you can use the test repository directly:

```bash
# Clone the test repository
git clone https://github.com/yourusername/ephemeral_poc.git
cd ephemeral_poc

# Run all tests
python test_runner.py all

# Run specific test types
python test_runner.py unit
python test_runner.py integration
python test_runner.py selenium
```

## 🔄 Updating the Test Suite

To update to a newer version of the test suite:

1. Update the `@main` reference in your workflow files to `@v1.0.0` (or specific version)
2. Or update the `TEST_BRANCH` environment variable to point to a different branch

## 📝 Adding Custom Tests

If you need to add custom tests to the ephemeral test suite:

1. Fork the [ephemeral_poc repository](https://github.com/yourusername/ephemeral_poc)
2. Add your custom tests
3. Update the composite action if needed
4. Reference your fork in the workflows

## 🐛 Troubleshooting

### Common Issues

1. **Tests Timeout**: Increase the timeout value in your workflow
2. **Docker Issues**: Ensure Docker is available in the GitHub Actions runner
3. **Test Failures**: Check the uploaded artifacts for detailed logs

### Debug Mode

Enable debug logging by adding this to your workflow:

```yaml
- name: Run Tests
  uses: yourusername/ephemeral_poc@main
  with:
    test-type: 'all'
  env:
    ACTIONS_STEP_DEBUG: true
```

## 📄 License

This project uses the Ephemeral POC Test Suite. See the [test repository](https://github.com/yourusername/ephemeral_poc) for license information.
