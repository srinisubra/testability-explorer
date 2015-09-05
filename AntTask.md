# Testability #

# Description #
Runs the Testability-Explorer from Ant.

# Parameters #

|**Attribute**|**Description**|**Required**|
|:------------|:--------------|:-----------|
|resultfile   |File to write results into|No, defaults to System.out.|
|errorfile    |File to write errors into.|No; defaults to restultfile value.|
|failureproperty|Set this property to true if build should fail based on findings, but don't stop the ant process immideately.|No          |
|print        |Prints the result in different formats and level of detail. **summary** prints package summary information. **detail** prints detail drill down information for each method call. **html** prints html summary information|No; defaults to summary.|
|printDepth   |Maximum depth to recurse and print costs of classes/methods that the classes under analysis depend on.|No; defaults to 0.|
|mincost      |Minimum Total Class cost required to print that class' metrics.|No; defaults to 1.|
|maxexcellentcost|Maximum Total Class cost to classify it as 'excellent'.|No; defaults 50.|
|maxacceptablecost|Maximum Total Class cost to classify it as 'acceptable'.|No; defaults to 100.|
|worstoffendercount|Print n number of worst offending classes.|No; defaults to 20.|
|whitelist    |Colon delimited whitelisted packages that will not count against you. Matches packages/classes starting with given values. (Always whitelists java.**).**|No.         |
|cyclomatic   |Cyclomatic cost multiplier. When computing the overall cost of the method the individual costs are added using the weighted average. This represents the weight of the cyclomatic cost.|No; defaults to 1|
|global       |Global state multiplier. When computing the overal cost of the method the individual costs are added using weighted average. This represents the weight of the global state cost.|No; defaults to restultfile value.|


# Parameters specified as nested elements #

**fileset**

http://jakarta.apache.org/ant/manual/CoreTypes/filterset.html filesets are used to get the list of jar files and/or class directories.

**Examples**

**Integrate Testability Ant task into Ant environment**

```
<taskdef name="testability" classname="com.google.ant.TestabilityTask"  classpath="PATH/ant-testability-explorer.jar;PATH/testability-explorer.jar"/>

```

**Use Testability-Explorer with default settings**

```
<target name="testability-simple">
  <testability>
    <classpath>
      <fileset dir="lib">
        <include name="project.jar"/>
      </fileset>
    </classpath>
   </testability>
</target>
```

**Use Testability-Explorer customized**

This example shows all possible arguments of the ant task including the failproperty. Instead of breaking the build on the spot this property is set and the build continues. The second snippet shows how the property can be read and used to break the build at a well defined timing.

```
<target name="testability-complex">
  <testability filter="" resultfile="testability.result.html" errorfile="testability.err.txt"
                printdepth="2" print="html" mincost="1" maxexcellentcost="50"
                maxacceptablecost="100" worstoffendercount="25" whitelist="com.thirdparty."
                cyclomatic="1" global="10" failproperty="testability.failproperty">
    <classpath>
      <fileset dir="lib">
         <include name="*.jar"/>
      </fileset>
      <pathelement location="build/classes"/>
    </classpath>
  </testability>
</target>
```
```
<target name="testability-break-build.test" depends="testability-complex">
   <fail if="testability.failproperty" message="testability failed"/>
</target>
```