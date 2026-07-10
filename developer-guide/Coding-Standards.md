<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Coding Standards</title>
  <style>
    body { font-family: Segoe UI, Arial, sans-serif; line-height: 1.6; margin: 20px; }
    h1, h2, h3 { color: #2b5797; }
    code { background-color: #f4f4f4; padding: 2px 4px; border-radius: 4px; }
    ul { margin-left: 20px; }
    pre { background-color: #f9f9f9; padding: 10px; border-radius: 6px; overflow-x: auto; }
  </style>
</head>
<body>

<h1>Coding Standards</h1>

<h2>Introduction</h2>
<p>Code consistency is not about making the codebase look uniform. It is about making implementation predictable.</p>
<p>A developer should be able to open any feature, understand how it is organized, and implement new functionality without introducing a different development style. Consistent code reduces review effort, simplifies debugging, and allows features to evolve independently.</p>
<p>This document defines the engineering standards followed throughout the TaskFlow codebase. These standards focus on implementation decisions rather than formatting conventions.</p>

<hr>

<h2>1. Organize Code by Feature</h2>
<p>TaskFlow organizes source code around business capabilities instead of technical layers.</p>
<pre>
lib/
└── features/
    ├── project/
    ├── sprint/
    ├── work_item/
    └── notification/
</pre>
<p>This structure keeps related code together, localizes changes, and clarifies ownership.</p>

<h3>Don’t Organize by Technical Layers</h3>
<pre>
lib/
├── screens/
├── widgets/
├── models/
├── repositories/
├── services/
└── utils/
</pre>
<p>This structure increases navigation cost and makes ownership unclear.</p>

<hr>

<h2>2. Keep Widgets Focused on Presentation</h2>
<p>Widgets describe the user interface. They should not describe how the application works.</p>
<ul>
  <li>Display application state</li>
  <li>Collect user input</li>
  <li>Trigger user actions</li>
  <li>Render feedback</li>
</ul>
<pre>
ElevatedButton(
  onPressed: () {
    context.read&lt;WorkItemCubit&gt;().create(request);
  },
  child: const Text('Create'),
)
</pre>
<p>Widgets should not validate business rules, send HTTP requests, or manage workflows.</p>

<hr>

<h2>3. Write Business Logic Once</h2>
<p>Business rules should have a single implementation. Duplicating rules across screens or helpers increases inconsistency.</p>
<p><em>If changing one business rule requires modifications in multiple files, the implementation probably has the wrong ownership boundary.</em></p>

<hr>

<h2>4. Build Small Widgets</h2>
<p>Large widgets mix layout, interaction, and state updates. Split widgets by responsibility.</p>
<pre>
ProjectScreen
 ├── ProjectHeader
 ├── ProjectSummary
 ├── SprintList
 ├── MemberList
 └── ActivityTimeline
</pre>

<hr>

<h2>5. Design for Change</h2>
<p>Application code changes continuously. The goal is to reduce the number of places that change.</p>
<ul>
  <li>If the workflow changes tomorrow, which files will I modify?</li>
  <li>Can another feature reuse this implementation?</li>
  <li>Is this behavior specific to this feature or shared?</li>
</ul>
<p>Simple code is preferred over clever abstractions written too early.</p>

<hr>

<h2>6. Follow Naming Conventions</h2>

<h3>File Naming</h3>
<pre>
work_item_screen.dart
project_repository.dart
notification_service.dart
</pre>

<h3>Class Naming</h3>
<pre>
class WorkItemScreen {}
class ProjectRepository {}
class UserProfile {}
</pre>

<h3>Variable and Method Naming</h3>
<pre>
final projectName = "TaskFlow";
void loadWorkItems() {}
</pre>

<h3>Boolean Naming</h3>
<pre>
bool isLoading;
bool hasPermission;
bool canEdit;
</pre>

<hr>

<h2>7. Handle Data, API, and State Correctly</h2>
<p>Follow clear responsibilities:</p>
<pre>
Widget
   |
Cubit / State Management
   |
Use Case / Business Logic
   |
Repository
   |
Data Source / API
</pre>

<h3>Repositories</h3>
<pre>
class WorkItemRepository {
  Future&lt;List&lt;WorkItem&gt;&gt; getWorkItems() async {
    final response = await api.fetchWorkItems();
    return response.map((item) => WorkItem.fromJson(item)).toList();
  }
}
</pre>

<h3>Error Handling</h3>
<pre>
throw NetworkException("Unable to load work items");
</pre>

<h3>State Management</h3>
<pre>
sealed class WorkItemState {}
class WorkItemLoading extends WorkItemState {}
class WorkItemLoaded extends WorkItemState {}
class WorkItemFailure extends WorkItemState {}
</pre>

<hr>

<h2>8. Maintain Code Quality</h2>
<ul>
  <li>Avoid duplicate logic</li>
  <li>Keep functions focused</li>
  <li>Avoid unnecessary abstraction</li>
</ul>

<h3>Review Checklist</h3>
<ul>
  <li>Feature-based structure followed?</li>
  <li>Business logic outside widgets?</li>
  <li>No duplicated logic?</li>
  <li>Clear, meaningful names?</li>
  <li>Errors handled consistently?</li>
  <li>Predictable states?</li>
</ul>

<hr>

<h2>9. Testing Standards</h2>
<ul>
  <li><strong>Unit tests:</strong> Validate business rules in isolation.</li>
  <li><strong>Integration tests:</strong> Ensure repositories interact correctly with APIs and storage.</li>
  <li><strong>UI tests:</strong> Verify widgets display state and handle input correctly.</li>
</ul>

<hr>

<h2>10. Documentation Standards</h2>
<ul>
  <li>Explain <em>why</em> code exists, not <em>what</em> it does.</li>
  <li>Provide docstrings for public classes and methods.</li>
  <li>Keep documentation updated when workflows change.</li>
  <li>Use plain language, active voice, and short sentences.</li>
</ul>

<hr>

<h2>11. Review Practices</h2>
<p>Peer reviews should focus on clarity, maintainability, and adherence to standards.</p>

</body>
</html>
