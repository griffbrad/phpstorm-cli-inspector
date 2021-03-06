phpstorm-cli-inspector
======================

A tool to make it easier to run PHPStorm inspections on the command-line.

PHPStorm offers a variety of very helpful inspections to help you catch errors
in your code.  Many people don't realize that you can also run these inspections
on the command-line without even opening the PHPStorm IDE.  This is possible
using the inspect.sh script that ships with PHPStorm, as documented here:

<http://www.jetbrains.com/phpstorm/webhelp/working-with-phpstorm-features-from-command-line.html>

Unfortunately, the arguments required to use this script are a bit tricky and
easy to get wrong.  On top of that, the output is just a bunch of XML files that
group the issues based upon the type of the problem that was found, rather than
the file the problem was found inside.


How does phpstorm-cli-inspector help?
-------------------------------------

To use PHPStorm's inspect.sh on a sub-directory of your project, you'd typically
need to run this command:

`/Applications/PhpStorm.app/bin/inspect.sh /delta/my-example-project/ /delta/my-example-project/.idea/inspectionProfiles/Project_Default.xml /tmp/output -d /delta/my-example-project/vendor/deltasystems/dewdrop/Dewdrop/Db/Dbdeploy/`

After running the inspections, you'd look through the XML files to see the results.

With phpstorm-inspect, this command becomes just:

`phpstorm-inspect /delta/my-example-project/vendor/deltasystems/dewdrop/Dewdrop/Db/Dbdeploy/`

And the errors are displayed immediately in your terminal, grouped by the name of 
file they were found in.  The path used in the phpstorm-inspect command can be any
sub-directory inside your PHPStorm project's files.  You don't have to manually
point it at your inspection profile, .idea folder, etc.  And the XML files are
cleaned up for you automatically when the command completes.
     
Here's some example output:

<pre>
$ ./bin/phpstorm-inspect /delta/my-example-project/vendor/deltasystems/dewdrop/Dewdrop/Db/Dbdeploy/
vendor/deltasystems/dewdrop/Dewdrop/Db/Dbdeploy/Changeset.php
=============================================================

* Line 169: PHPDoc comment doesn't contain all necessary @throws tag(s)

vendor/deltasystems/dewdrop/Dewdrop/Db/Dbdeploy/CliExec.php
===========================================================

* Line 198: PHPDoc comment doesn't contain all necessary @throws tag(s)
</pre>


Installing phpstorm-inspect
---------------------------

You install phpstorm-inspect with Composer:

<pre>
composer global require 'griffbrad/phpstorm-cli-inspector:>=1.0'
sudo ln -s ~/.composer/vendor/bin/phpstorm-inspect /usr/local/bin/phpstorm-inspect
</pre>

Note that the above command assumes you've installed Composer globally as
described here:

<https://getcomposer.org/doc/00-intro.md#globally>


FAQ
---

*Does it work in Windows?*

No.  If any Windows users want to make it work, I'd be glad to accept pull requests.

*What if PHPStorm is in a non-standard location?*

You can add a .phpstorm-inspect.ini configuration file to your home folder.  You can
specify two things in that configuration: "inspectPath", which would point to the
inspect.sh that ships with PHPStorm, and "excludePatterns", which should be regular
expressions that will exclude matching problem descriptions.

