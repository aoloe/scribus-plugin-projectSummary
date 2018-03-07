# Scribus Project Summary

Create a text summary of the content and settings of a Scribus file.

- It has been asked multiple times to have a way to get a project summary / description / printing instructions
  - But I can't find the tickets in the bug tracker.
  - [Preflight Verifier: Design a report for printing with the the new file tester](https://bugs.scribus.net/view.php?id=1770) by mrdocs
- InDesign produces an `Instructions.txt` file when packaging the project
  - some people are not happy that they're not able to avoid the creation of the file
    - <https://indesign.uservoice.com/forums/601021-adobe-indesign-feature-requests/suggestions/20365246-make-the-instructions-txt-file-choosable-when-pa>
  - some people want to control what gets included
  - some people want to pre-populate some of the files
  - here is an [`instructions.txt`](https://github.com/nwtn/hackathought/blob/master/Resources/design%20working%20files/Thought%20Spot%204%20Folder-1/Instructions.txt) file.
- Optionally, produce the summary when collecting for output
- This plugin produces
  - a yaml file:
    - a machine readable file
    - a human readable file
  - with the following fields:
    - Name of .sla file (main file)
    - File save date
    - Packaging date
    - Contact person for the printing workflow (Company name, Contact person, Phone, Email)
    - Fonts (embedded or not)
    - Colors
    - Images
    - Transparencies
    - Document Size and orientation
      - Page size
      - Margins
      - Layout (single / facing pages)
    - Pdf export settings
- use https://github.com/jbeder/yaml-cpp to produce yaml (is it really needed?)
  - how to emit yaml: <https://github.com/jbeder/yaml-cpp/wiki/How-To-Emit-YAML>
  - inspiration on use it with qt:
    - https://github.com/simonmeaden/qyaml
    - https://gist.github.com/brcha/d392b2fe5f1e427cc8a6
    - https://stackoverflow.com/questions/3342355/save-a-yaml-emitter-content-to-a-file-with-yaml-cpp : a few lines to output a yaml
  - or use it as a dependency to the deb package?
    - <https://stackoverflow.com/questions/23656282/fatal-error-yaml-cpp-yaml-h-no-such-file-or-directory>

## Todo

- allow to pass the filename to the plugin and avoid the save as dialog.
- eventually use the last path instead of the current path...

## Notes

### YAML usages

```cpp
{
    YAML::Emitter emitter;
    emitter << "Hello world!";
}

{
    YAML::Node node;
    node["key"] = "value";
    node["seq"].push_back("first element");
    node["seq"].push_back("second element");
    // qDebug() << node;
    std::stringstream ss;
    ss << node;
    qDebug() << QString::fromStdString(ss.str());
}

{
    YAML::Emitter out;
    out << YAML::BeginMap;
    out << YAML::Key << "name";
    out << YAML::Value << "Barack Obama";
    out << YAML::Key << "children";
    out << YAML::Value << YAML::BeginSeq << "Sasha" << "Malia" << YAML::EndSeq;
    out << YAML::EndMap;
    qDebug() << out.c_str();
}

{
    std::vector <int> squares;
    squares.push_back(1);
    squares.push_back(4);
    squares.push_back(9);
    squares.push_back(16);

    std::map <std::string, int> ages;
    ages["Daniel"] = 26;
    ages["Jesse"] = 24;

    YAML::Emitter out;
    out << YAML::BeginSeq;
    out << YAML::Flow << squares;
    out << ages;
    out << YAML::EndSeq;
    qDebug() << out.c_str();
}
```
