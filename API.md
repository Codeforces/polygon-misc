# Polygon API Documentation

## What's new

- 2026-06-03: Added [`problem.note`](#problemnote) and [`problem.saveNote`](#problemsavenote). The [Problem](#problem) object now returns `note`.
- 2026-06-01: Added [`problem.deleteTest`](#problemdeletetest) with all-or-nothing pre-checks and structured failure details.
- 2026-05-29: [`problem.checkerTests`](#problemcheckertests) now returns `runVerdict` and `runComment` when checker test run results are available.
- 2026-05-29: [`problem.validatorTests`](#problemvalidatortests) now returns `runVerdict` and `runComment` when validator test run results are available.
- 2026-05-29: Added [`problem.renderStatements`](#problemrenderstatements) to render statement and tutorial HTML/PDF files from the current working copy.
- 2026-05-29: Added [`problem.cautions`](#problemcautions) to return structured problem cautions, package-readiness issues, latest package warnings and cached AI correction tips.
- 2026-05-29: [`problem.updateInfo`](#problemupdateinfo) now validates that `timeLimit` is divisible by 50.
- 2026-05-29: Added documentation for [`problem.clearScript`](#problemclearscript), [`problem.previewTests`](#problempreviewtests), [`problem.enableTreatPointsFromCheckerAsPercent`](#problemenabletreatpointsfromcheckeraspercent) and [`problem.viewStatementResource`](#problemviewstatementresource).

## Contents

- [How to download a problem or contest](#how-to-download-a-problem-or-contest)
- [Introduction](#introduction)
- [Authorization](#authorization)
- [Methods](#methods)
    - [problems.list](#problemslist)
    - [problem.create](#problemcreate)
  - [Methods for problems](#methods-for-problems)
    - [problem.info](#probleminfo)
    - [problem.updateInfo](#problemupdateinfo)
    - [problem.note](#problemnote)
    - [problem.saveNote](#problemsavenote)
    - [problem.updateWorkingCopy](#problemupdateworkingcopy)
    - [problem.discardWorkingCopy](#problemdiscardworkingcopy)
    - [problem.commitChanges](#problemcommitchanges)
    - [problem.statements](#problemstatements)
    - [problem.renderStatements](#problemrenderstatements)
    - [problem.saveStatement](#problemsavestatement)
    - [problem.statementResources](#problemstatementresources)
    - [problem.viewStatementResource](#problemviewstatementresource)
    - [problem.saveStatementResource](#problemsavestatementresource)
    - [problem.checker](#problemchecker)
    - [problem.validator](#problemvalidator)
    - [problem.extraValidators](#problemextravalidators)
    - [problem.interactor](#probleminteractor)
    - [problem.validatorTests](#problemvalidatortests)
    - [problem.saveValidatorTest](#problemsavevalidatortest)
    - [problem.checkerTests](#problemcheckertests)
    - [problem.saveCheckerTest](#problemsavecheckertest)
    - [problem.files](#problemfiles)
    - [problem.solutions](#problemsolutions)
    - [problem.viewFile](#problemviewfile)
    - [problem.viewSolution](#problemviewsolution)
    - [problem.script](#problemscript)
    - [problem.clearScript](#problemclearscript)
    - [problem.tests](#problemtests)
    - [problem.previewTests](#problempreviewtests)
    - [problem.testInput](#problemtestinput)
    - [problem.testAnswer](#problemtestanswer)
    - [problem.setValidator](#problemsetvalidator)
    - [problem.setChecker](#problemsetchecker)
    - [problem.setInteractor](#problemsetinteractor)
    - [problem.saveFile](#problemsavefile)
    - [problem.saveSolution](#problemsavesolution)
    - [problem.editSolutionExtraTags](#problemeditsolutionextratags)
    - [problem.saveScript](#problemsavescript)
    - [problem.saveTest](#problemsavetest)
    - [problem.deleteTest](#problemdeletetest)
    - [problem.setTestGroup](#problemsettestgroup)
    - [problem.enableGroups](#problemenablegroups)
    - [problem.enablePoints](#problemenablepoints)
    - [problem.enableTreatPointsFromCheckerAsPercent](#problemenabletreatpointsfromcheckeraspercent)
    - [problem.viewTestGroup](#problemviewtestgroup)
    - [problem.saveTestGroup](#problemsavetestgroup)
    - [problem.viewTags](#problemviewtags)
    - [problem.saveTags](#problemsavetags)
    - [problem.viewGeneralDescription](#problemviewgeneraldescription)
    - [problem.saveGeneralDescription](#problemsavegeneraldescription)
    - [problem.viewGeneralTutorial](#problemviewgeneraltutorial)
    - [problem.saveGeneralTutorial](#problemsavegeneraltutorial)
    - [problem.packages](#problempackages)
    - [problem.package](#problempackage)
    - [problem.buildPackage](#problembuildpackage)
    - [problem.cautions](#problemcautions)
  - [Contest methods](#contest-methods)
    - [contest.problems](#contestproblems)
- [Return objects](#return-objects)
    - [Problem](#problem)
    - [ProblemInfo](#probleminfo-1)
    - [CommitResult](#commitresult)
    - [Statement](#statement)
    - [RenderStatements](#renderstatements)
    - [RenderedStatement](#renderedstatement)
    - [RenderResult](#renderresult)
    - [File](#file)
    - [ResourceAdvancedProperties](#resourceadvancedproperties)
    - [Solution](#solution)
    - [Test](#test)
    - [DeleteTestsResult](#deletetestsresult)
    - [TestDeletionFailure](#testdeletionfailure)
    - [TestPreview](#testpreview)
    - [TestGroup](#testgroup)
    - [Package](#package)
    - [ValidatorTest](#validatortest)
    - [CheckerTest](#checkertest)
    - [ProblemCautions](#problemcautions-1)
    - [Caution](#caution)
    - [PackageReadinessIssue](#packagereadinessissue)
    - [AiTips](#aitips)
    - [StatementAiTip](#statementaitip)
    - [SourceAiTip](#sourceaitip)

# How to download a problem or contest
Use HTTP POST-requests:

- Use problem URL and POST-parameters `<login>`, `<password>` and optionals `<revision>`, `<type>` to download a zip-file with problem package (you can use type=windows or type=linux). If the problem has the pin code, provide an optional POST-parameter `<pin>`. Example:

    wget --post-data=login=vtapochkin&password=???
    https://polygon.codeforces.com/p85dIBF/mmirzayanov/a-plus-b

- Use problem URL + /problem.xml and POST-parameters `<login>`, `<password>` and optional `<revision>` to download a problem descriptor (much more faster, use it to check if new revision has been released). If the problem has the pin code, provide an optional POST-parameter `<pin>`.  Example:

    wget --post-data=login=vtapochkin&password=???
    https://polygon.codeforces.com/p85dIBF/mmirzayanov/a-plus-b/problem.xml

- Use 'c/ContestUID/contest.xml' and POST-parameters `<login>`, `<password>` to download a contest descriptor. If the contest has the pin code, provide an optional POST-parameter `<pin>`.  Example:

    wget --post-data=login=vtapochkin&password=???
    https://polygon.codeforces.com/c/50431a121273b7e31f4200e7/contest.xml

# Introduction
To access Polygon API you just send a HTTP-request to address `https://polygon.codeforces.com/api/{methodName}` with method-specific parameters.

Most method calls return a JSON-object (if not specified otherwise) with three possible fields: status, comment and result.

- Status is either "OK" or "FAILED".
- If status is "FAILED" then comment contains the reason why the request failed. If status is "OK", then there is no comment.
- If status is "OK" then result contains method-dependent JSON-element which will be described for each method separately. If status is "FAILED", then there is no result unless the method explicitly documents a failure result.

# Authorization
Using Polygon API requires authorization. To authorize, you will need an API key, which can be generated on settings page. Each API key has two parameters: *key* and *secret*. To use the key you must add following parameters to your request.

1. `apiKey` - must be equal to *key*.
2. `time` - current time in UNIX format (e.g., System.currentTimeMillis()/1000). If the difference between server time and time, specified in parameter, will be greater than 5 minutes, request will be denied.
3. `apiSig` - signature to ensure that you know both *key* and *secret*. The first six characters of the *apiSig* parameter can be arbitrary. We recommend to choose them at random for each request. Let's denote them as *rand*. The rest of the parameter is hexadecimal representation of SHA-512 hash-code of the following string: *`<rand>`/`<methodName>`?param1=value1&param2=value2...&paramN=valueN#`<secret>`* where *(param_1, value_1), (param_2, value_2), ..., (param_n, value_n)* are all the request parameters (including *apiKey*, *time*, but excluding *apiSig*) with corresponding values, sorted lexicographically first by *param_i*, then by *value_i*.

# Methods

### problems.list
Returns a list of problems, available to the user, according to search parameters.

#### Parameters:
- `showDeleted` - *bool, optional* - show/hide deleted problems (defaults to *false*)
- `id` - problem id, optional
- `name` - problem name, optional
- `owner` - problem owner login, optional

#### Returns:
A list of [*Problem*](#problem) objects.

### problem.create
Create a new empty problem. Returns a created [Problem](#problem).

#### Parameters:
- `name` - name of problem being created

## Methods for problems
To access problem-specific API methods, add a *problemId* parameter to your request. The user must have access to the problem. Methods require at least READ access unless stated otherwise; `problem.commitChanges`, `problem.saveNote` and `problem.buildPackage` require WRITE access. Translators may use only methods explicitly marked as available to translators. If the problem has the pin code, add the *pin* parameter to your request.

The problem-specific methods available to translators are `problem.info`, `problem.statements`, `problem.renderStatements`, `problem.saveStatement`, `problem.statementResources`, `problem.viewStatementResource` and `problem.saveStatementResource`.

### problem.info
Returns a [ProblemInfo](#probleminfo) object.

### problem.updateInfo
Update problem info. All parameters are optional.

#### Parameters:
- `inputFile` - problem's input file, optional; if specified, must be 1..64 characters, valid text and UTF-8
- `outputFile` - problem's output file, optional; if specified, must be 1..64 characters, valid text and UTF-8
- `interactive` - *boolean* - is problem interactive
- `wellFormed` - *boolean* - is tests well-formed
- `skipDuplicatedTestsValidation` - *boolean* - whether to skip checking that tests are unique
- `timeLimit` - problem's time limit in milliseconds, from 250 to 15000, must be divisible by 50
- `memoryLimit` - problem's memory limit in MB, from 4 to 1024

If both the effective input and output file names are equal ignoring case, the method returns status "FAILED".

### problem.note
Returns problem note. This note is a problem-level user note and is not related to statement `notes`.

#### Parameters:
None

#### Returns:
A string - the problem note, or an empty string if no note is set.

### problem.saveNote
Saves problem note. This method updates the stored problem note immediately; it does not modify the working copy and does not require `problem.commitChanges`.

#### Parameters:
- `note` - *string* - the problem note to save, up to 50 characters, valid text and UTF-8. Use an empty string to clear the note.

### problem.updateWorkingCopy
Updates working copy.

### problem.discardWorkingCopy
Discards working copy.

### problem.commitChanges
Commits problem changes. All parameters are optional.

#### Parameters:
- `minorChanges` - *boolean* - if *true,* no email notification will be sent
- `message` - problem's commit message

#### Returns:
A [CommitResult](#commitresult) object. If there are no changes, the top-level status is "OK" and the result contains `committed=false`, `conflictOccurred=false` and `message="No changes"`. If commit fails, the top-level status is "FAILED" and `comment` contains the failure message.

### problem.statements
Returns a map from language to a [Statement](#statement) object for that language.

### problem.renderStatements
Renders statements and tutorials from the current working copy. For each available statement language, the method returns independent HTML and PDF render results. Tutorials are returned only for languages where the current working copy has non-empty tutorial content.

The method requires READ access and is available to translators. If one HTML or PDF render fails or times out, the method still returns status "OK" and puts the error into that particular [RenderResult](#renderresult). Each individual HTML/PDF render is limited to 1 minute.

If the whole request does not finish in 4 minutes, the method returns status "FAILED" with comment "Statement rendering did not finish in 4 minutes. Please repeat the request in 4 minutes."

#### Parameters:
- `includeContent` - boolean, optional - if *true*, successful render results include `contentBase64`; defaults to *false*

#### Returns:
A [RenderStatements](#renderstatements) object.

### problem.saveStatement
Update or create a problem's statement. All parameters except for *lang* are optional.

#### Parameters:
- `lang` (required) - statement's language
- `encoding` - statement's encoding
- `name` - problem's name in statement's language
- `legend` - statement legend/main statement text
- `input` - problem's input format
- `output` - problem's output format
- `scoring` - problem's scoring
- `interaction` - problem's interaction protocol (only for interactive problems)
- `notes` - statement notes
- `tutorial` - problem's tutorial

### problem.statementResources
Returns a list of statement resources for the problem.

#### Parameters:
None

#### Returns:
A list of [*File*](#file) objects.

### problem.viewStatementResource
Returns statement resource file. In case of success, this method does not return JSON, instead it returns the requested file with the corresponding mime-type set.

This method is available to translators.

#### Parameters:
- `name` - statement resource file name

### problem.saveStatementResource
Add or edit statement resource file.

This method is available to translators.

#### Parameters:
- `checkExisting` - boolean, optional - if *true*, only adding files is allowed
- `name` - file name
- `file` - file content

### problem.checker
Returns the name of the currently set checker.

### problem.validator
Returns the name of the currently set validator.

### problem.extraValidators
Returns an array containing the names of the currently set extra validators.

### problem.interactor
Returns the name of the currently set interactor.

### problem.validatorTests
Returns a list of validator tests for the problem.

#### Parameters:
None

#### Returns:
A list of [*ValidatorTest*](#validatortest) objects.

### problem.saveValidatorTest
Add or edit validator test. In case of editing, all parameters except for *testIndex* are optional. In case of adding, all parameters except for *testIndex*, *testInput* and *testVerdict* are optional.

#### Parameters:
- `checkExisting` - boolean, optional - if *true*, only adding validator's test is allowed
- `testVerdict` - VALID/INVALID - validator test verdict
- `testIndex` - index of a validator test
- `testInput` - input of a validator test
- `testGroup` - *optional* - test group (groups should be enabled for the testset)
- `testset` - *optional* - testset name

### problem.checkerTests
Returns a list of checker tests for the problem.

#### Parameters:
None

#### Returns:
A list of [*CheckerTest*](#checkertest) objects.

### problem.saveCheckerTest
Adds or edits checker test. In case of editing, all parameters except for *testIndex* are optional. In case of adding, all parameters except for *testIndex*, *testInput*, *testAnswer*, *testOutput* and *testVerdict* are optional.

#### Parameters:
- `checkExisting` - boolean, optional - if *true*, only adding checker test is allowed
- `testVerdict` - OK/WRONG_ANSWER/PRESENTATION_ERROR/CRASHED - checker's test verdict
- `testIndex` - index of a checker test
- `testInput` - input of a checker test
- `testOutput` - output of a checker test
- `testAnswer` - answer of a checker test

### problem.files
Returns the list of resource, source and aux files. Method returns a JSON object with three fields: *resourceFiles, sourceFiles* and *auxFiles*, each of them is a list of [*File*](#file) objects.

### problem.solutions
Returns the list of [*Solution*](#solution) objects.

### problem.viewFile
Returns resource, source or aux file. In case of success, this method does not return JSON, instead it returns plain view of the file with the corresponding mime-type set.

#### Parameters:
- `type` - resource/aux/source - requested file's type
- `name` - file name

### problem.viewSolution
Returns solution file. In case of success, this method does not return JSON, instead in returns plain view of the solution file.

#### Parameters:
- `name` - solution's name

### problem.script
Returns script for generating tests. In case of success, this method does not return JSON, instead it returns plain view of the script.

#### Parameters:
- `testset` - testset for which the script is requested

### problem.clearScript
Clears script for generating tests in the specified testset.

#### Parameters:
- `testset` - testset for which the script should be cleared

### problem.tests
Returns tests for the given testset

#### Parameters:
- `testset` - testset for which tests are requested
- `noInputs` - boolean, optional - if *true*, returns tests without input

#### Returns:
A list of [*Test*](#test) objects.

### problem.previewTests
Returns cached or currently available test previews for tests in the specified testset. If a preview is missing or stale, the method may start preview generation and return a [TestPreview](#testpreview) object without `input` and `answer`; repeat the request later to get generated data.

#### Parameters:
- `testset` - testset for which tests should be previewed

#### Returns:
A list of [*TestPreview*](#testpreview) objects.

### problem.testInput
Returns generated test input. In case of success, this method does not return JSON, instead it returns plain view.

#### Parameters:
- `testset` - testset of the test
- `testIndex` - index of the test

### problem.testAnswer
Returns generated test answer. In case of success, this method does not return JSON, instead it returns plain view.

#### Parameters:
- `testset` - testset of the test
- `testIndex` - index of the test

### problem.setValidator
Update validator.

#### Parameters:
- `validator` - name of the validator (one of source files)

### problem.setChecker
Update checker.

#### Parameters:
- `checker` - name of the checker (one of source files)

### problem.setInteractor
Update interactor.

#### Parameters:
- `interactor` - name of the interactor (one of source files)

### problem.saveFile
Add or edit resource, source or aux file. In case of editing, all parameters except for *type* and *name* are optional.

#### Parameters:
- `checkExisting` - boolean, optional - if *true*, only adding files is allowed
- `type` - file type (*resource*/*source* or *aux*)
- `name` - file name
- `file` - file content
- `sourceType` - *optional* - source type (in case of a source file)
In case of type=resource there some possible additional parameters:
- `forTypes` - *optional* - semicolon separated list of applicable file types (see ResourceAdvancedProperties)
- `stages` - *optional* - semicolon separated list of values COMPILE or RUN, meaning the phase when the resource is applicable (currently only *stages=*COMPILE is supported)
- `assets` - *optional* - semicolon separated list of values VALIDATOR, INTERACTOR, CHECKER, SOLUTION, meaning the asset types the resource is applicable (currently only *assets=*SOLUTION is supported)

The parameters *forTypes, stages, assets* can be present only together (it means, all of them are absent or all of them are used at the same time). They can be used only for *type=resource.*

To delete ResourceAdvancedProperties of a resource file pass empty "*forTypes="*.

### problem.saveSolution
Add or edit solution. In case of editing, all parameters except for *name* are optional.

#### Parameters:
- `checkExisting` - boolean, optional - if *true*, only adding solutions is allowed
- `name` - solution name
- `file` - solution content
- `sourceType` - *optional* - source type
- `tag` - solution's tag: *MA* (Main), *OK*, *RJ*, *TL*, *TO* (Time Limit or Accepted), *TM* (Time Limit or Memory Limit), *WA*, *PE*, *ML, NR* (Do not run), *RE*.

### problem.editSolutionExtraTags
Add or remove testset or test group extra tag for solution.

#### Parameters:
- `remove` - boolean - if *true* - remove extra tag, if *false* - add extra tag.
- `name` - solution name
- `testset` - *optional* - testset name for which edit extra tag
- `testGroup` - optional - test group name for which edit extra tag
Exactly one from *testset* and *testGroup* should be specified
- `tag` - *optional* - when you add extra tag - solution's extra tag: *OK*, *RJ*, *TL*, *TO* (Time Limit or Accepted), *TM* (Time Limit or Memory Limit), *WA*, *PE*, *ML, NR* (Do not run), *RE*.

### problem.saveScript
Edit script.

#### Parameters:
- `testset` - testset of the script
- `source` - script source

### problem.saveTest
Add or edit test. In case of editing, all parameters except for *testset* and *testIndex* are optional.

#### Parameters:
- `checkExisting` - boolean, optional - if *true*, only adding new test is allowed
- `testset` - testset of the test
- `testIndex` - index of the test
- `testInput` - test input
- `testGroup` - *optional* - test group (groups should be enabled for the testset)
- `testPoints` - *optional* - test points (points should be enabled for the problem); non-negative double value less than 10^9 with at most 2 digits after a decimal point
- `testDescription` - optional - test description
- `testUseInStatements` - bool, optional - whether to use test in statements
- `testInputForStatements` - optional - test input for viewing in the statements
- `testOutputForStatements` - optional - test output for viewing in the statements
- `verifyInputOutputForStatements` - bool, optional - whether to verify input and output for statements

### problem.deleteTest
Delete one or more tests from a testset.

The method first checks all requested tests and then deletes them. It returns "OK" only if every requested test was deleted. If at least one requested test can not be deleted during the pre-check, the method returns "FAILED" with a [DeleteTestsResult](#deletetestsresult) object, comment "Some tests can not be deleted." and does not delete any tests. `DELETE_FAILED` may occur after a successful pre-check during the actual deletion; if it occurs, the method returns comment "Some tests failed during deletion; problem state may be partially modified." and some earlier tests may already have been deleted.

#### Parameters:
- `testset` - testset of the test(s)
- `testIndex` - index of the test; you can specify multiple parameters with the same name *testIndex* to delete many tests from the same testset
- `testIndices` - list of test indices, separated by a comma. It is an alternative for *testIndex*; use only one of these two ways

### problem.setTestGroup
Set test group for one or more tests. It expects that for specified testset test groups are enabled.

#### Parameters:
- `testset` - testset of the test(s)
- `testGroup` - test group name to set
- `testIndex` - index of the test, you can specify multiple parameters with the same name *testIndex* to set test group to many tests of the same testset at the same time.
- `testIndices` - list of test indices, separated by a comma. It's alternative for *testIndex,* you should use only one from these two ways

### problem.enableGroups
Enable or disable test groups for the specified testset.

#### Parameters:
- `testset` - testset to enable or disable groups
- `enable` - bool - if it is true test groups become enabled, else test groups become disabled

### problem.enablePoints
Enable or disable test points for the problem.

#### Parameters:
- `enable` - bool - if it is true test points become enabled, else test points become disabled

### problem.enableTreatPointsFromCheckerAsPercent
Enable or disable treating points returned by checker as percents.

If `enable` is *true* while test points are disabled for the problem, the method returns status "FAILED".

#### Parameters:
- `enable` - bool - if it is true checker points are treated as percents, else this behavior is disabled

### problem.viewTestGroup
Returns test groups for the specified testset.

#### Parameters:
- `testset` - testset name
- `group` - optional - test group name

#### Returns:
A list of [*TestGroup*](#testgroup) objects.

### problem.saveTestGroup
Saves test group. Use if only to save a test group. If you want to create new test group, just add new test with such test group.

#### Parameters:
- `testset` - testset name
- `group` - test group name
- `pointsPolicy` - *optional* - COMPLETE_GROUP or EACH_TEST (leaves old value if no specified)
- `feedbackPolicy` - *optional* - NONE, POINTS, ICPC or COMPLETE (leaves old value if no specified)
- `dependencies` - optional - string of group names from which group should depends on separated by a comma

### problem.viewTags
Returns tags for the problem.

#### Parameters:
None

#### Returns:
A list of strings - tags for the problem.

### problem.saveTags
Saves tags for the problem. Existed tags will be replaced by new tags.

#### Parameters:
- `tags` - string of tags, separated by a comma. If you specified several same tags will be add only one of them.

### problem.viewGeneralDescription
Returns problem general description.

#### Parameters:
None

#### Returns:
A string - the problem general description.

### problem.saveGeneralDescription
Saves problem general description.

#### Parameters:
- `description` - *string* - the problem general description to save. The description may be empty.

### problem.viewGeneralTutorial
Returns problem general tutorial.

#### Parameters:
None

#### Returns:
A string - the problem general  tutorial.

### problem.saveGeneralTutorial
Saves problem general tutorial.

#### Parameters:
- `tutorial` - *string* - the problem general tutorial to save. The tutorial may be empty.

### problem.packages
Returns a list of [*Package*](#package) objects - list all packages available for the problem.

#### Parameters:
None

### problem.package
Download a package as a zip-archive.

#### Parameters:
- `packageId` - package id
- `type` - string, optional - type of the package: "standard", "linux" or "windows". Standard packages don't contain generated tests, but contain windows executables and scripts for windows and linux via wine. Linux packages contain generated tests, but don't contain compiled binaries. Windows packages contain generated tests and compiled binaries for Windows. If not provided, "standard" is used.

### problem.buildPackage
Starts to build a new [*Package*](#package).

#### Parameters:
- `full` - bool - defines whether to build full package, contains "standard", "linux" and "windows" packages, or only "standard"
Standard packages don't contain generated tests, but contain windows executables and scripts for windows and linux via wine. Linux packages contain generated tests, but don't contain compiled binaries. Windows packages contain generated tests and compiled binaries for Windows.
- `verify` - bool - if that parameter is true all solutions will be invoked on all tests to be sure that tags are valid. Stress tests will run the checker to verify its credibility.

### problem.cautions
Returns structured warnings and package-readiness issues for the problem. Also returns cached "Correction tips from AI", if available.

The method inspects the current working copy. The four caution arrays `common`, `statement`, `structure` and `issues` are always present and may be empty. If both a missing checker/validator caution and the corresponding missing tests caution are detected, the redundant missing tests caution is omitted: `NO_CHECKER_TESTS` is omitted when `NO_CHECKER` is present, and `NO_VALIDATOR_TESTS` is omitted when `NO_VALIDATOR` is present.

AI tips are read from Polygon cache only; this method does not start new AI requests. If problem metadata cannot be built, the method returns status "FAILED".

The method requires READ access and is not available to translators.

#### Parameters:
None

#### Returns:
A [ProblemCautions](#problemcautions-1) object.

## Contest methods
To access contest methods, you have to include *contestId* parameter in your request. If the contest has the pin code, you need to include the parameter *pin* in your request.

### contest.problems
Returns a list of [*Problem*](#problem) objects - problems of the contest.

# Return objects

### Problem
Represents a polygon problem.
- `id` - problem id
- `owner` - problem owner handle
- `name` - problem name
- `note` - problem note set by users with write access (may be absent)
- `deleted` - boolean - is a problem deleted
- `favourite` - boolean - is a problem in user's favourites
- `accessType` - *READ/WRITE/OWNER* - user's access type for the problem
- `revision` - current problem revision
- `workingCopyRevision` - current working copy revision (may be absent)
- `latestPackage` - latest revision with package available (may be absent)
- `modified` - is the problem modified (does the working copy have uncommitted modifications)

### ProblemInfo
Represents the problem's general information.
- `inputFile` - problem's input file
- `outputFile` - problem's output file
- `interactive` - *boolean* - is problem interactive
- `wellFormed` - *boolean* - is tests well-formed
- `skipDuplicatedTestsValidation` - *boolean* - whether checking that tests are unique is skipped
- `timeLimit` - problem's time limit in milliseconds
- `memoryLimit` - problem's memory limit in MB

### CommitResult
Represents result of a `problem.commitChanges` call.
- `committed` - boolean, whether changes were committed
- `conflictOccurred` - boolean, whether a conflict occurred while updating the working copy during commit processing
- `message` - human-readable commit result message

### Statement
Represents a problem's statement.
- `encoding` - statement's encoding
- `name` - problem's name in statement's language
- `legend` - statement legend/main statement text
- `input` - problem's input format
- `output` - problem's output format
- `scoring` - problem's scoring
- `interaction` - problem's interaction protocol (only for interactive problems)
- `notes` - statement notes
- `tutorial` - problem's tutorial

### RenderStatements
Represents rendered statements and tutorials for the current working copy.
- `revision` - working copy revision used for rendering
- `renderingTimeSeconds` - unix time in seconds when rendering result was produced
- `statements` - list of [RenderedStatement](#renderedstatement) objects, one for each statement language; always present, may be empty
- `tutorials` - list of [RenderedStatement](#renderedstatement) objects for languages with non-empty tutorial content; always present, may be empty

### RenderedStatement
Represents rendered files for one language.
- `language` - statement language
- `html` - [RenderResult](#renderresult) object for HTML render
- `pdf` - [RenderResult](#renderresult) object for PDF render

### RenderResult
Represents one HTML or PDF render result.
- `status` - OK/FAILED
- `sha256` - SHA-256 hex digest of the rendered file; present when `status` is OK
- `sizeBytes` - rendered file size in bytes; present when `status` is OK
- `contentBase64` - rendered file content encoded as Base64; present only when `status` is OK and `includeContent` is *true*
- `message` - render error message; present when `status` is FAILED

### File
Represents a resource, source or aux file.
- `name` - file name
- `modificationTimeSeconds` - file's modification time in unix format
- `length` - file length
- `sourceType` - (present only for source files) - source file type
- `resourceAdvancedProperties` - (may be absent) Problem resource files can have extra property called *resourceAdvancedProperties* of type ResourceAdvancedProperties

### ResourceAdvancedProperties
Represents special properties of resource files. Basically, they stand for compile- or run-time resources for specific file types and asset types. The most important application is IOI-style graders.
Example: {"forTypes":"cpp.*","main":false,"stages":["COMPILE"],"assets":["SOLUTION"]}
- `forTypes` - colon or semicolon separated list of file types this resource if applied, wildcards are supported (example: "cpp.*" or "pascal.*;java.11")
- `main` - currently reserved to be false,
- `stages` - array of possible string values COMPILE or RUN, meaning the phase when the resource is applicable,
- `assets` - array of possible string values VALIDATOR, INTERACTOR, CHECKER, SOLUTION, meaning the asset types the resource is applicable.

### Solution
Represents a problem solution.
- `name` - solution name
- `modificationTimeSeconds` - solution's modification time in unix format
- `length` - solution length
- `sourceType` - source file type
- `tag` - solution tag

### Test
Represents a test for the problem.
- `index` - test index
- `manual` - boolean - whether test is manual of generated
- `input` - lossy UTF-8 string view of test input (absent for generated tests)
- `inputBase64` - exact Base64-encoded test input bytes (absent for generated tests and for manual tests without input)
- `description` - test description (may be absent)
- `useInStatements` - boolean - whether test is included in statements
- `scriptLine` - script line for generating test (absent for manual tests)
- `group` - test group (may be absent)
- `points` - test points (may be absent)
- `inputForStatement` - input for statements (may be absent)
- `outputForStatement` - output for statements (may be absent)
- `verifyInputOutputForStatements` - boolean - whether to verify input and output for statements (may be absent)

### DeleteTestsResult
Represents structured failure details for `problem.deleteTest`.
- `failures` - list of [TestDeletionFailure](#testdeletionfailure) objects; contains one item for each requested test that can not be deleted

### TestDeletionFailure
Represents a test that can not be deleted.
- `index` - requested test index
- `reason` - DUPLICATE/NOT_FOUND/FREEMARKER_SCRIPT_TEST/DELETE_FAILED
- `message` - human-readable failure message

### TestPreview
Represents preview data for one test.
- `id` - internal preview id (may be absent)
- `session` - working copy session
- `creationTime` - preview creation time, serialized as a legacy Gson date string (may be absent)
- `updateTime` - preview update time, serialized as a legacy Gson date string (may be absent)
- `problemId` - problem id
- `testset` - testset name
- `testIndex` - test index
- `description` - test description (may be absent)
- `useInStatements` - boolean - whether test is included in statements
- `input` - generated preview input; may be absent while preview generation is pending; may contain an error message if preview generation failed
- `inputSha1` - SHA-1 hash of generated preview input (may be absent)
- `answer` - generated preview answer; may be absent while preview generation is pending; may contain an error message if preview generation failed
- `changeAfterLastGenerate` - boolean, whether the test changed after the last preview generation
- `testOverviewLog` - test overview log (may be absent)

### TestGroup
Represents a test group in testset.
- `name` - test group name
- `pointsPolicy` - test group points policy, COMPLETE_GROUP for the complete group points policy, EACH_TEST for the each test points policy
- `feedbackPolicy` - test group feedback policy, COMPLETE for the complete feedback policy, ICPC for the first error feedback policy, POINTS for the only points feedback policy, NONE for no feedback policy.
- `dependencies` - list of group names from which this group depends on (may be empty)

### Package
Represents a package.
- `id` - package's id
- `revision` - revision of the problem for which the package was created
- `creationTimeSeconds` - package's creation time in unix format
- `state` - PENDING/RUNNING/READY/FAILED package's state
- `comment` - comment for the package
- `type` - type of the package: "standard", "linux" or "windows". Standard packages don't contain generated tests, but contain windows executables and scripts for windows and linux via wine. Linux packages contain generated tests, but don't contain compiled binaries. Windows packages contain generated tests and compiled binaries for Windows.

### ValidatorTest
Represents a validator's test for the problem.
- `index` - validator test index
- `input` - validator test input
- `expectedVerdict` - INVALID/VALID - expected verdict for the validator test
- `testset` - validator test set (may be absent)
- `group` - validator test group (may be absent)
- `runVerdict` - VALID/INVALID/IN_QUEUE/CANT_RUN - validator test run verdict, if available; absent if the test has not been run
- `runComment` - validator test run comment, if available; absent if unavailable

### CheckerTest
Represents a checker's test for the problem.
- `index` - checker test index
- `input` - checker test input
- `output` - checker test output
- `answer` - checker test answer
- `expectedVerdict` - OK/WRONG_ANSWER/CRASHED/PRESENTATION_ERROR - expected verdict for the checker's test
- `runVerdict` - checker test run verdict, if available; completed runs return raw checker interop verdict name; `IN_QUEUE` means the run has not finished yet; absent if the test has not been run
- `runComment` - checker test run comment, if available; absent if unavailable

### ProblemCautions
Represents structured cautions for the problem.
- `common` - list of [Caution](#caution) objects with COMMON category; always present, may be empty
- `statement` - list of [Caution](#caution) objects with STATEMENT category; always present, may be empty
- `structure` - list of [Caution](#caution) objects with STRUCTURE category; always present, may be empty
- `issues` - list of [Caution](#caution) objects with ISSUES category; always present, may be empty
- `packageReadinessIssues` - list of [PackageReadinessIssue](#packagereadinessissue) objects found by package validation; always present, may be empty
- `latestPackageWarnings` - list of package verification warning strings from the latest READY package; always present, may be empty
- `ai` - [AiTips](#aitips) object with cached AI correction tips; always present

### Caution
Represents one problem caution.
- `type` - caution type, for example NO_TAGS, NO_CHECKER, TESTSET_SEEMS_TO_BE_INCOMPLETE or OPENED_ISSUES
- `severity` - SOFT/HARD
- `category` - COMMON/STATEMENT/STRUCTURE/ISSUES
- `message` - human-readable caution message
- `parameters` - list of string parameters used to format the caution message; may be empty

### PackageReadinessIssue
Represents a pre-package state issue collected by package validation.
- `type` - package validation exception type, for example HAS_MODIFICATIONS, WORKING_COPY_IS_OUTDATED, CHECKER_IS_NOT_SET, NO_TESTS or INVALID_TEST_SCRIPT
- `reason` - optional exception context, for example a source file name or testset name; may be absent
- `message` - human-readable formatted package validation message; fixed for simple types such as HAS_MODIFICATIONS, CHECKER_IS_NOT_SET and NO_TESTS, and may include `reason` and/or an internal exception message for types such as INVALID_TEST_SCRIPT

`problem.cautions` uses fast package validation, so compile-only package checks are not included in `packageReadinessIssues`.

### AiTips
Represents cached "Correction tips from AI" for the problem.
- `disabled` - boolean, whether AI tips are disabled for the problem; always present
- `statements` - list of [StatementAiTip](#statementaitip) objects for statement languages with cached AI correction data; always present, may be empty
- `validator` - [SourceAiTip](#sourceaitip) object with cached validator review comment; may be absent if there is no cached useful comment or if validator review is not applicable
- `checker` - [SourceAiTip](#sourceaitip) object with cached checker review comment; may be absent if there is no cached useful comment or if checker review is not applicable

If `disabled` is *true*, `statements` is empty and `validator` and `checker` are absent. Internal UI confidence flags for AI tips are not returned.

### StatementAiTip
Represents a cached AI correction tip for one statement language.
- `language` - statement language
- `source` - normalized statement text used for the cached AI request; may be large
- `suggestion` - suggested corrected statement text; absent when no suggestion is available, including when `processing` is *true*
- `processing` - boolean, whether the cached AI request is still being processed

If `processing` is *true*, call `problem.cautions` later to check whether the suggestion has appeared.

### SourceAiTip
Represents a cached AI review comment for a source file.
- `name` - source file name
- `comment` - raw cached AI review comment; may be large
