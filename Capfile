<?xml version="1.0" encoding="UTF-8"?>
<!-- two parameters are send throug phing: projectName and profile  -->
<project name="${projectName}" basedir="." default="build:main">

    <!-- Config files -->
    <property name="dir.config"                   value="${project.basedir}/" />
    <property name="config.behat"                 value="${dir.config}/config/behat.yml" />

    <!-- Build paths -->
    <property name="dir.build"                    value="${project.basedir}/build" />
    <property name="dir.reports"                  value="${dir.build}/reports" />
    <property name="dir.reports.test"             value="${dir.reports}/test" />
    <property name="dir.logs"                     value="${dir.build}/logs" />
    <property name="dir.logs.jira"                value="${dir.logs}/behat-jira" />

    <!-- Build settings -->
    <property name="option.composer.mode"         value="install" />
    <property name="option.feature.jira"          value="http://jira.fullsix.com/" />
    <property name="option.feature.local"         value="features" />
    <property name="profile"                      value="" override="no"/>

    <!-- BUILD TASKS -->

    <!-- Main (default task) -->
    <target name="build:main"
            depends="build:clean, build:prepare, build:test"
            description="Run all test and build everything"/>

    <!-- Clean previous build files -->
    <target name="build:clean"
            description="Clean previous build files">

        <delete dir="${dir.build}" verbose="true" />

    </target>

    <!-- Prepare build (performed by each build:* task when called as standalone) -->
    <target name="build:prepare"
            depends="build:clean"
            description="Prepare build">

        <mkdir dir="${dir.build}" />

    </target>

    <!-- Test Project -->
    <target name="build:test"
            description="Perform all tests"
            depends="build:prepare, test:prepare, test:behat:local, test:behat:jira"/>

    <!-- TEST SECTION -->

    <!-- Prepare test environment (performed by each test:* task when called as standalone) -->
    <target name="test:prepare"
            description="Prepare the test environment">

        <echo msg="Prepare test reports and logs directory" />
        <mkdir dir="${dir.reports.test}" />
        <mkdir dir="${dir.logs.jira}" />

        <echo msg="Installing/Updating vendors" />
        <exec command="composer ${option.composer.mode}" passthru="true"/>

        <property name="option.profile.argument"      value="--profile "/>
        <if>
            <equals arg1="${profile}" arg2="" />
            <then>
                <echo msg="Using no profile" />
                <property name="option.profile.argument"      value="" override="true"/>
            </then>
            <else>
                <echo msg="Using profile: ${profile}" />
            </else>
        </if>
    </target>

    <!-- Execute behat local features -->
    <target name="test:behat:local"
            description="Perform behat local features"
            depends="test:prepare">

        <echo msg="Running Behat local features" />
        <exec executable="bin/behat" logoutput="true">
            <arg line="--config ${config.behat}" />
            <arg line="${option.profile.argument}${profile}" />
            <arg line="-f junit" />
            <arg line="--out ${dir.reports.test}" />
            <arg line="${option.feature.local}" />
        </exec>
    </target>

    <!-- Execute behat jira features -->
    <target name="test:behat:jira"
            description="Perform behat jira features"
            depends="test:prepare">

        <echo msg="Running Behat jira features" />
        <exec executable="bin/behat" logoutput="true">
            <arg line="--config ${config.behat}" />
            <arg line="${option.profile.argument}${profile}" />
            <arg line="-f junit" />
            <arg line="--out ${dir.reports.test}" />
            <arg line="${option.feature.jira}" />
        </exec>
    </target>

</project>