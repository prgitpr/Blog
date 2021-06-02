1. Install the “Checkstyle-IDEA” plugin from the IntelliJ plugin repository.
2. Configure the plugin by going to Settings -> Other Settings -> Checkstyle.
3. Set the “Scan Scope” to “Only Java sources (including tests)”.
4. Select 8.14 in the “Checkstyle Version” dropdown and click apply. This step is important, don’t skip it!
5. In the “Configuration File” pane, add a new configuration using the plus icon:
6. Set the “Description” to “Flink”.
7. Select “Use a local Checkstyle file”, and point it to "tools/maven/checkstyle.xml" within your repository.
8. Check the box for “Store relative to project location”, and click “Next”.
9. Configure the “checkstyle.suppressions.file” property value to "suppressions.xml", and click “Next”, then “Finish”.
10. Select “Flink” as the only active configuration file, and click “Apply” and “OK”.
11. Checkstyle will now give warnings in the editor for any Checkstyle violations.