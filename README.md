# 🚀 API Testing with Postman, Newman & Jenkins CI/CD

[![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-red.svg)](https://www.jenkins.io/)
[![Newman](https://img.shields.io/badge/Newman-CLI-orange.svg)](https://www.npmjs.com/package/newman)
[![Postman](https://img.shields.io/badge/Postman-API%20Testing-orange.svg)](https://www.postman.com/)

Professional API test automation framework for **Swagger Petstore API**, demonstrating automated testing in a complete CI/CD pipeline with Jenkins integration and JUnit reporting.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Test Coverage](#-test-coverage)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Jenkins CI/CD Pipeline](#-jenkins-cicd-pipeline)
- [Test Reports](#-test-reports)
- [Understanding the Tests](#-understanding-the-tests)
- [Author](#-author)

---

## 🎯 Project Overview

This project showcases a **production-ready API testing framework** that validates the complete lifecycle of the Swagger Petstore API, including Pet management, Store orders, and User operations. All tests are executed automatically through Jenkins CI/CD pipeline with comprehensive reporting.

**Target API:** [Swagger Petstore API v2](https://petstore.swagger.io/v2)

**Key Highlights:**
- ✅ 18 automated API test cases
- ✅ Full CRUD operation coverage
- ✅ Response validation & data verification
- ✅ Performance testing (response time checks)
- ✅ Negative testing (404 scenarios)
- ✅ Pre-request scripts for dynamic test data
- ✅ Environment variable management
- ✅ Jenkins pipeline integration
- ✅ JUnit XML test reports

---

## ✨ Features

### API Testing Capabilities
- **Comprehensive Validations**
  - Status code verification
  - Response body validation
  - Data integrity checks
  - Performance assertions (response time < 200ms)
  - Error message validation

### Test Data Management
- **Dynamic Test Data Generation**
  - Random pet names, IDs, and status
  - Environment variables for data persistence
  - Test data isolation between runs

### CI/CD Integration
- **Automated Execution**
  - Jenkins pipeline orchestration
  - Automatic test triggering on code changes
  - Build success/failure based on test results
  - JUnit test result publishing

---

## 🛠 Tech Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| **Postman** | Latest | API test design and validation |
| **Newman** | Latest | CLI test execution |
| **Jenkins** | 2.x+ | CI/CD orchestration |
| **Node.js** | 14+ | Runtime for Newman |
| **JUnit** | XML Format | Test result reporting |

---

## 🧪 Test Coverage

### Pet Module (8 tests)
| Test Name | Method | Validates |
|-----------|--------|-----------|
| FindPetByStatus | GET | Query parameter filtering, status code |
| AddPetToTheStore | POST | Pet creation, response data accuracy |
| UpdateExistingPet | PUT | Pet update, data persistence |
| UpdatePetWithFormData | POST | Form data submission |
| UploadAPetPhoto | POST | File upload functionality |
| FindPetById | GET | Pet retrieval by ID, data verification |
| DeleteAPet | DELETE | Pet deletion |
| FindPetById_404 | GET | **Negative Test** - 404 error handling |

### Store Module (4 tests)
| Test Name | Method | Validates |
|-----------|--------|-----------|
| PetInventoryByStatus | GET | Inventory retrieval |
| PlaceAnOrder | POST | Order creation, **performance test** (< 200ms) |
| GetOrderById | GET | Order retrieval, ID verification |
| DeleteOrderById | DELETE | Order deletion |

### User Module (6 tests)
| Test Name | Method | Validates |
|-----------|--------|-----------|
| CreateAUser | POST | User registration |
| UpdateUserByUsername | PUT | User data update |
| GetUserByUsername | GET | User retrieval, credential verification |
| UserLogin | GET | Authentication |
| UserLogout | GET | Session management |
| DeleteUser | DELETE | User deletion |

**Total: 18 API Test Cases**

---

## 📁 Project Structure

```
api-testing-postman-newman-jenkins/
│
├── postman/
│   ├── PetShopApi.postman_collection.json    # Test collection (18 tests)
│   ├── PetShopApi.postman_environment.json   # Environment variables
│   └── test_image.png                        # Test file for upload
│
├── newman/
│   └── PetShopApi.xml                        # Generated JUnit report
│
├── Jenkinsfile                                # CI/CD pipeline definition
└── README.md                                  # This file
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** (v14 or higher)
- **npm** (comes with Node.js)
- **Jenkins** (for CI/CD execution)
- **Git** (for version control)

### Local Setup (Optional)

If you want to run tests locally for development:

```bash
# 1. Clone the repository
git clone https://github.com/TayfunYaman/api-testing-postman-newman-jenkins.git
cd api-testing-postman-newman-jenkins

# 2. Install Newman globally
npm install -g newman

# 3. Run tests
newman run postman/PetShopApi.postman_collection.json \
  -e postman/PetShopApi.postman_environment.json \
  --reporters cli,junit \
  --reporter-junit-export newman/PetShopApi.xml

# 4. View results in console
```

### Import to Postman (For Test Development)

1. Open Postman
2. Click **Import** → **File**
3. Select `postman/PetShopApi.postman_collection.json`
4. Import environment: `postman/PetShopApi.postman_environment.json`
5. Select **PetShopApi** environment from dropdown
6. Run collection or individual requests

---

## 🔄 Jenkins CI/CD Pipeline

### Pipeline Configuration

The Jenkins pipeline is defined in `Jenkinsfile` and consists of two main stages:

#### Stage 1: Install Newman
```groovy
stage('Install Newman') {
    steps {
        sh 'npm install newman'
    }
}
```
Installs Newman CLI runner for executing Postman collections.

#### Stage 2: Run API Tests
```groovy
stage('Run API Tests') {
    steps {
        sh '''
          mkdir -p newman
          npx newman run postman/PetShopApi.postman_collection.json \
            -e postman/PetShopApi.postman_environment.json \
            --reporters cli,junit \
            --reporter-junit-export newman/PetShopApi.xml
        '''
    }
}
```
Executes all 18 API tests and generates JUnit XML report.

#### Post-Build Actions
```groovy
post {
    always {
        junit 'newman/PetShopApi.xml'
    }
}
```
Publishes test results to Jenkins, marking build as:
- ✅ **SUCCESS** - All tests passed
- ❌ **FAILURE** - One or more tests failed

### Jenkins Setup

1. **Create New Pipeline Job**
   - Jenkins Dashboard → New Item
   - Enter job name: `PetStore-API-Tests`
   - Select: **Pipeline**
   - Click **OK**

2. **Configure Pipeline**
   - Pipeline Definition: **Pipeline script from SCM**
   - SCM: **Git**
   - Repository URL: `https://github.com/TayfunYaman/api-testing-postman-newman-jenkins.git`
   - Script Path: `Jenkinsfile`

3. **Build Triggers** (Optional)
   - Poll SCM: `H/5 * * * *` (every 5 minutes)
   - GitHub hook trigger for GITScm polling

4. **Save & Build**
   - Click **Build Now**
   - View **Console Output** for execution logs
   - Check **Test Result** for detailed report

---

## 📊 Test Reports

### JUnit XML Report

After each Jenkins build, test results are available at:
```
Newman/PetShopApi.xml
```

**Report includes:**
- Total test count: 18
- Passed tests
- Failed tests (if any)
- Test execution time
- Individual test details

### Viewing Results in Jenkins

1. Navigate to build page
2. Click **Test Result** in left sidebar
3. View:
   - Test summary (pass/fail counts)
   - Individual test results
   - Failure messages and stack traces
   - Test duration
   - Historical trends

### Console Output

Full test execution logs available in Jenkins **Console Output**, showing:
- Newman execution details
- Each test case result
- Response times
- Assertion results
- Error messages (if any)

---

## 🔍 Understanding the Tests

### Test Structure

Each test request includes:

1. **Pre-request Scripts** (if applicable)
   - Generate random test data
   - Set environment variables
   ```javascript
   let petNames = ["Buddy", "Max", "Rocky", "Bella", "Lucy"];
   pm.environment.set("petName", petNames[Math.floor(Math.random() * petNames.length)]);
   ```

2. **Request Execution**
   - HTTP method (GET, POST, PUT, DELETE)
   - Request URL with environment variables
   - Headers and body (if applicable)

3. **Test Assertions**
   - Status code validation
   - Response body verification
   - Data integrity checks
   ```javascript
   pm.test("Are Pet Infos Correct", function () {
       pm.expect(response.id).is.eql(pm.environment.get("petId"));
       pm.expect(response.name).is.eql(pm.environment.get("petName"));
       pm.expect(response.status).is.eql(pm.environment.get("petStatus"));
   });
   ```

### Example: AddPetToTheStore Test

**Pre-request:**
- Generates random pet name, status, and ID
- Stores in environment variables

**Request:**
```json
POST /pet
{
  "id": {{petId}},
  "name": "{{petName}}",
  "status": "{{petStatus}}"
}
```

**Assertions:**
- Validates response contains correct ID
- Verifies pet name matches request
- Confirms status was set correctly
- Checks status code is 200

### Performance Test Example

**PlaceAnOrder test** includes performance validation:
```javascript
pm.test("Response time is less than 200ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(200);
});
```

### Negative Test Example

**FindPetById_404 test** validates error handling:
```javascript
pm.test("Status code is 404", function () {
    pm.response.to.have.status(404);
});

pm.test("Error message is correct", function () {
    pm.expect(response.message).to.eql("Pet not found");
});
```

---

## 🎯 Why This Project Demonstrates Value

### For Employers

This project shows:
- ✅ **API Testing Expertise** - Comprehensive test coverage across all HTTP methods
- ✅ **Automation Skills** - Fully automated test execution pipeline
- ✅ **CI/CD Knowledge** - Jenkins pipeline integration with reporting
- ✅ **Best Practices** - Environment variables, dynamic data, assertions
- ✅ **Quality Focus** - Data validation, performance checks, error handling

### Real-World Application

This framework demonstrates skills used in:
- Production API testing
- Continuous Integration workflows
- Automated regression testing
- API contract validation
- Performance monitoring

---

## 🔧 Customization

### Adding New Tests

1. Open collection in Postman
2. Add new request to appropriate folder
3. Add test scripts:
   ```javascript
   pm.test("Your test name", function () {
       pm.response.to.have.status(200);
       // Add more assertions
   });
   ```
4. Export collection
5. Commit to repository
6. Jenkins automatically runs new tests

### Environment Variables

Modify `PetShopApi.postman_environment.json`:
```json
{
  "key": "BaseUrl",
  "value": "https://petstore.swagger.io/v2",
  "enabled": true
}
```

---

## 📝 Best Practices Demonstrated

1. **Separation of Concerns**
   - Collection file for tests
   - Environment file for variables
   - Jenkinsfile for CI/CD

2. **Data Management**
   - Dynamic test data generation
   - Environment variables for data persistence
   - No hardcoded values

3. **Comprehensive Validation**
   - Status code checks
   - Response body validation
   - Data integrity verification
   - Performance assertions

4. **CI/CD Integration**
   - Automated execution
   - Clear reporting
   - Build status based on test results

---

## 🚀 Future Enhancements

Potential improvements for expanded portfolio:
- [ ] Add more negative test scenarios (400, 401, 403)
- [ ] Implement data-driven testing with CSV
- [ ] Add HTML report generation (newman-reporter-html)
- [ ] Include API contract testing
- [ ] Add security tests (authentication, authorization)
- [ ] Integrate with test management tools
- [ ] Add Slack/Email notifications on failure

---

## 👤 Author

**Tayfun Yaman**

- GitHub: [@TayfunYaman](https://github.com/TayfunYaman)
- LinkedIn: [Tayfun Yaman](https://www.linkedin.com/in/tayfunyaman)
- Email: tayfunyaman@example.com

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Swagger Petstore API** - Test API provided by Swagger
- **Postman/Newman** - API testing and automation tools
- **Jenkins** - CI/CD platform

---

<div align="center">

**⭐ If you find this project useful, please consider giving it a star! ⭐**

*Demonstrating professional API testing skills for QA automation roles*

</div>
