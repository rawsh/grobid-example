This project illustrates how to embed Grobid ([grobid](https://raw.github.com/kermitt2/grobid)) in your Java application. 

You need to install first and build Grobid, see:  
## [Build the project](https://github.com/kermitt2/grobid/wiki/Build-the-project)

via maven, this will deploy the Grobid artifact in your local maven repository. This will also create a jar file under: grobid/grobid-core/target/grobid-core-`<current version>`.jar

Copy the Grobid jar library under grobid-test/lib :

> copy target/grobid-core-`<current version>`.jar `path_to_grobid_test`/grobid-test/lib

The paths to __grobid-home__ and to the property __grobid.properties__ file must be changed in the project property file:  grobid-example/grobid-example.properties according to your installation, for instance: 

		grobid_example.pGrobidHome=/Users/lopez/grobid/grobid-home
		grobid_example.pGrobidProperties=/Users/lopez/grobid/grobid-home/config/grobid.properties

You can then test the example project:

> mvn test

We then describe here how Grobid is used in this example project. You can use this project as a skeleton to implement your own Grobid process or as an illustration to use Grobid in an existing/new project. 

## Building with maven

When using maven, if you want to call Grobid API, you need to include in your pom file the path to the Grobid jar file, for instance as follow (replace `0.2.10` by the valid `<current version>`):

	<dependency>
	    <groupId>org.grobid.core</groupId>
	    <artifactId>grobid</artifactId>
	    <version>0.2.10</version>
	    <scope>system</scope>
	    <systemPath>${project.basedir}/lib/grobid-core-0.2.10.jar</systemPath>
	</dependency>

## API call

When using Grobid, you have to initiate a context with the path to the Grobid resources, the following class give an example of usage:

        import org.grobid.core.*;
        import org.grobid.core.data.*;
        import org.grobid.core.factory.*;
        import org.grobid.core.mock.*;
        import org.grobid.core.utilities.*;
        import org.grobid.core.engines.Engine;
        ...
        String pdfPath = "mypdffile.pdf";
        ...
	try {
			String pGrobidHome = "/Users/lopez/grobid/grobid-home";
			String pGrobidProperties = "/Users/lopez/grobid/grobid-home/config/grobid.properties";

			MockContext.setInitialContext(pGrobidHome, pGrobidProperties);		
			GrobidProperties.getInstance();

			System.out.println(">>>>>>>> GROBID_HOME="+GrobidProperties.get_GROBID_HOME_PATH());

			Engine engine = GrobidFactory.getInstance().createEngine();

			// Biblio object for the result
			BiblioItem resHeader = new BiblioItem();
			String tei = engine.processHeader(pdfPath, false, resHeader);
		} 
		catch (Exception e) {
			// If an exception is generated, print a stack trace
			e.printStackTrace();
		} 
		finally {
			try {
				MockContext.destroyInitialContext();
			} 
			catch (Exception e) {
				e.printStackTrace();
			}
		}

The context paths (pGrobidHome and pGrobidProperties) can be set by a property file, or for a web application by a web.xml file (see for instance grobid-service in the [grobid](https://raw.github.com/kermitt2/grobid) project).


