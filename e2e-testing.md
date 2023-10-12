# E2E Testing Style Guide <!-- omit in toc -->

[go/e2e-testing-style-guide](http://go/e2e-testing-style-guide)

- [1\. Factory classes](#1-factory-classes)
  - [1.1. Use Factory classes for Entities only](#11-use-factory-classes-for-entities-only)
- [2\. Page objects](#2-page-objects)
  - [2.1. Define page locators outside the constructor](#21-define-page-locators-outside-the-constructor)
  - [2.2. Parametrized page URLs defining](#22-parametrized-page-urls-defining)
  - [2.3. Use Components to re-use common page element groups](#23-use-components-to-re-use-common-page-element-groups)
  - [2.4. Do not use dynamic values in allure step text](#24-do-not-use-dynamic-values-in-allure-step-text)
  - [2.5 Use fill instead of type](#25-use-fill-instead-of-type)
- [3\. Test scenarios](#3-test-scenarios)
  - [3.1. Use underscore separator for test folders naming](#31-use-underscore-separator-for-test-folders-naming)
  - [3.2. Use folders for test suite organization](#32-use-folders-for-test-suite-organization)
  - [3.3. Use one spec file per one test](#33-use-one-spec-file-per-one-test)
  - [3.4. Before block should not contain common test data preparation](#34-before-block-should-not-contain-common-test-data-preparation)
  - [3.5. Test block should only contain test steps_and_assertions](#35-test-block-should-only-contain-test-steps-and-assertions)
- [4\. Assertions](#4-assertions)
  - [4.1 One assertion per one test step method](#41-one-assertion-per-one-test-step-method)
  - [4.2 Use expect for assertions](#42-use-expect-for-assertions)


1\. Factory classes
--------------

#### 1.1. Use Factory classes for Entities only

The ```FactoryItem``` class should be extended for each new Entity like User, Course, Event, StudentGroup class.
For the tables which contains additional information for the Item it's recommended to create a ```link``` method.


```typescript
// ✅ recommended
export class User extends FactoryItem {

  public constructor(
    apiClient: SeedAPI,
    options: Options,
  ) {
    super(apiClient);
    ...
  }

  async linkToCourse(
    courseId: number,
    status: CourseUserStatus,
  ): Promise<void> {
    await this.api.seedItemIfNotExist(
      FactoryItemType.CourseUsers,
      {
        courseId,
        userId: this.id,
        status,
      },
    );
  }
}
```

2\. Page objects
--------------

#### 2.1. Define page locators outside the constructor

Please define locators in the page object classes and components using the below example.
There could be different approaches, but we agreed to follow a single style within the organization.

```typescript
// ❌ not recommended
export class SignInPage extends BasePage{
  private readonly emailField: Locator;

  constructor(page: Page) {
      super(page);
      this.emailField = page.getByTestId('sign-in-user-email');
  }
}

// ✅ recommended
export class SignInPage extends BasePage{
  private readonly emailField = this.page.getByTestId('sign-in-user-email');

  constructor(page: Page) {
    super(page);
  }
}
```



#### 2.2. Parametrized page URLs defining

Use the parametrized ROUTES to define the parametrized page object URLs.

```typescript
// ❌ not recommended
interface Options {
  chatId: number;
}

export class ChatPage extends LoggedInBasePage {

  constructor(
    page: Page,
    options: Options,
  ) {
    super(page);

    this.url = `${ROUTES.chat}\\${options.chatId}`;
  }
}

// ✅ recommended
interface Options {
  chatId: number;
}

export class ChatPage extends LoggedInBasePage {

  constructor(
    page: Page,
    options: Options,
  ) {
    super(page);

    this.url = ROUTES.chat(options.chatId).index;
  }
}
```


#### 2.3. Use Components to re-use common page element groups


Do not repeat the code for page elements.
If you defined some common elements from the header, footer, sideBar, or some popups that appear on several pages - make sure to define them in the corresponding component class.



#### 2.4. Do not use dynamic values in allure step text


Do not use dynamic values in allure step text becuase that way new test case will be added to the Allure TestOps on each test re-run.

```typescript
// ❌ not recommended
async typeCourseName(name: string): Promise<void> {
  await test.step(`Type course name ${name}`, async () => {
    await this.courseDropwDown.type(name);
  });
}


// ✅ recommended
async typeCourseName(name: string): Promise<void> {
  await test.step('Type course name', async () => {
    await this.courseDropwDown.type(name);
  });
}

```

#### 2.5. Use fill instead of type


Use `fill()` instead of `type()` method, as `type()` is deprecated in [playwright](https://playwright.dev/docs/api/class-locator#locator-type).

```typescript
// ❌ not recommended

await this.textField.type(text);


// ✅ recommended

await this.textField.fill(text);
```

3\. Test scenarios
--------------

#### 3.1. Use underscore separator for test folders naming


Use the underscore separator for test folder names. Never use spaces in the folders and file names.


```typescript
// ❌ not recommended
- tests
    - e2e
        - Admin Tools
            - Homework-Review-Plugin


// ✅ recommended
- tests
   - e2e
        - Admin_Tools
            - Homework_Review_Plugin

```


#### 3.2. Use folders for test suite organization


Do not add test spec files from different suites to one folder. Create separate folder for each test suite.

```typescript
// ❌ not recommended
- tests
  - e2e
    - LMS_Editor
      - Courses
          - shouldBeCreatedWithOnlyRequiredFields.spec.ts
          - shouldBeCreatedWithAllFields.spec.ts
          - shouldEditOnlyRequiredFields.spec.ts
          - shouldEditAllFields.spec.ts

// ✅ recommended
- tests
  - e2e
    - LMS_Editor
      - Courses
        - New_course
            - shouldBeCreatedWithOnlyRequiredFields.spec.ts
            - shouldBeCreatedWithAllFields.spec.ts
        - Edit_course
            - shouldEditOnlyRequiredFields.spec.ts
            - shouldEditAllFields.spec.ts
```

#### 3.3. Use one spec file per one test


Do not add several tests into one spec file. Each spec file should normally have one e2e test scenario.
The exception can be done for the parametrized tests.

```typescript
// ❌ not recommended
test.describe(`New application form`, () => {
    test('should allow to open the page')
    test('should allow to be submitted')
    test('should show the success message')
  }
)


// ✅ recommended
test.describe(`New application form`, () => {
  test('should allow to successfully submit the application by new user')
  }
)

```


#### 3.4. Before block should not contain common test data preparation

The before test block in the test spec file should generally contain only calling the fixtures and defining the shortcut constants for better test readability.
In case one need to define some additional data preparation (for example seeding some data) - one need to do this in a separate methods and fixtures which can be re-used in other tests.



```typescript
// ❌ not recommended
test.beforeEach((
  {
    page,
    newProfessionUA,
    techCheckTopicInNewCourse,
  },
) => {
  const techCheckQuestion
    = new TechCheckQuestion(
    seedAPI,
    { techCheckTopic: techCheckTopicInNewCourse },
  );

  await techCheckQuestion.createWithEnTranslate();

  courseName = newProfessionUA.postpaid.nameShortCode;
  questionEn = techCheckQuestion.enTranslation;

  questionEditorPage = new QuestionsEditorPage(page);
});


// ✅ recommended
test.beforeEach((
  {
    page,
    newProfessionUA,
    techCheckQuestionInTopicInNewCourse,
  },
) => {
  courseName = newProfessionUA.postpaid.nameShortCode;
  questionEn = techCheckQuestionInTopicInNewCourse.enTranslation;

  questionEditorPage = new QuestionsEditorPage(page);
});

```

#### 3.5. Test block should only contain test steps and assertions

Test block should contain only test-step or test-assertion methods - these are page object methods wrapped with allure steps and clearly describing user performed step or assertion.
Do not use page locators or locator action methods directly in the test spec file - add page-object method instead.

```typescript
// ❌ not recommended
test('should allow to successfully create course',
  async () => {
  ...
    await createCoursePage.courseField.fill(name);
  ...
    await createCoursePage.waitForFlashMessage('Course_succesfully_created');
  });


// ✅ recommended
export class CreateCoursePage {

  async fillCourseName(name: string): Promise<void> {
    await test.step(`Fill the course name`, async () => {
      await this.courseField.fill(name);
    });
  }

  async assertCourseCreatedMessage(): Promise<void> {
    await test.step(`Assert course created message`, async () => {
      await this.waitForFlashMessage('Course_succesfully_created');
    });
  }
}

test('should allow to successfully create course',
  async () => {
    ...
    await createCoursePage.fillCourseName(name);
    ...
    await createCoursePage.assertCourseCreatedMessage();
  });

```

4\. Assertions
--------------

#### 4.1. One assertion per one test step method


Create a separate method for each test assertion. This allows to read all the test steps at one glance and allows to automatically import clear test case steps to allure.


```typescript
// ❌ not recommended
export class QuestionsEditorPage {
  async assertQuestionAdded(question: string): Promise<void> {
    await test.step(`Assert question added`, async () => {
      await this.assertFlashMessage('editor.question_successfully_created');

      await expect(this.questionInList.getByText(question)).toBeVisible();
    });
  }
}

// ✅ recommended
export class QuestionsEditorPage {

  async assertQuestionCreatedSuccessMessage(): Promise<void> {
    await this.assertFlashMessage('editor.question_successfully_created');
  }

  async assertQuestionIsPresentInTheList(question: string): Promise<void> {
    await test.step(`Assert question is present in the list`, async () => {
      await expect(this.questionInList.getByText(question)).toBeVisible();
    });
  }
}

```

#### 4.2. Use expect for assertions


Always use expect for assertions.

```typescript
// ❌ not recommended
export class QuestionsEditorPage {

  async assertQuestionIsPresentInTheList(question: string): Promise<void> {
    await test.step(`Assert question is present in the list`, async () => {
      await this.questionInList.getByText(question).isVisible();
    });
  }
}

// ✅ recommended
export class QuestionsEditorPage {

  async assertQuestionIsPresentInTheList(question: string): Promise<void> {
    await test.step(`Assert question is present in the list`, async () => {
      await expect(this.questionInList.getByText(question)).toBeVisible();
    });
  }
}

```