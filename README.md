scalaxb
=======

scalaxb is an XML data-binding tool for Scala that supports W3C XML 
Schema (xsd) as the input file.

Status
------

This is still at pre-ALPHA state, and many things don't work.
I'd really appreciate if you could run it against your favorite xsd
file and let me know the result.

Installation
------------

scalaxb is tested only under Scala 2.8. You can install it using sbaz:

    $ sudo sbaz install scalaxb

and upgrade it if you've installed it before:

    $ sudo sbaz upgrade

or build from source:

    $ git clone git://github.com/eed3si9n/scalaxb.git scalaxb
    $ cd scalaxb
    $ sbt sbaz

See the file called INSTALL for details.

Usage
-----

    $ scalaxb [options] <schema_file>...

      -d <directory> | --outdir <directory>
            generated files will go into <directory>
      -p <package> | --package <package>
            specifies the target package
      -p:<namespaceURI>=<package> | --package:<namespaceURI>=<package>
            specifies the target package for <namespaceURI>
      --wrap-contents <complexType>
            wraps inner contents into a seperate case class
      -v | --verbose
            be extra verbose
      <schema_file>
            input schema to be converted

Documents
---------

Further info is available at [scalaxb.org](http://scalaxb.org/).

Example
-------

Suppose you have address.xsd:

    <schema targetNamespace="http://www.example.com/IPO"
            xmlns="http://www.w3.org/2001/XMLSchema"
            xmlns:ipo="http://www.example.com/IPO">
      <complexType name="Address">
        <sequence>
          <element name="name"   type="string"/>
          <element name="street" type="string"/>
          <element name="city"   type="string"/>
        </sequence>
      </complexType>
    </schema>

You then run the following:

    $ scalaxb address.xsd
  
You get address.scala that contains case classes that can convert XML 
documents conforming to the address.xsd into a case class object, and turn it back again
into XML document:

    // Generated by <a href="http://scalaxb.org/">scalaxb</a>.
    import org.scalaxb.rt

    case class Address(name: String,
      street: String,
      city: String)

    object Address extends rt.ElemNameParser[Address] {
      val targetNamespace = "http://www.example.com/IPO"
  
      def parser(node: scala.xml.Node): Parser[Address] =
        (rt.ElemName(targetNamespace, "name")) ~ 
          (rt.ElemName(targetNamespace, "street")) ~ 
          (rt.ElemName(targetNamespace, "city")) ^^
            { case p1 ~ 
          p2 ~ 
          p3 => Address(p1.text,
          p2.text,
          p3.text) }
        
      def toXML(__obj: Address, __namespace: String, __elementLabel: String): scala.xml.NodeSeq = {
        var __scope: scala.xml.NamespaceBinding = scala.xml.TopScope
        __scope = scala.xml.NamespaceBinding("xsi", "http://www.w3.org/2001/XMLSchema-instance", __scope)
        __scope = scala.xml.NamespaceBinding("ipo", "http://www.example.com/IPO", __scope)
        __scope = scala.xml.NamespaceBinding(null, "http://www.example.com/IPO", __scope)
        val node = toXML(__obj, __namespace, __elementLabel, __scope)
        node match {
          case elem: scala.xml.Elem =>
            elem % new scala.xml.PrefixedAttribute(__scope.getPrefix(rt.Helper.XSI_URL),
              "type",
              "ipo:Address", elem.attributes)
          case _ => node
        }
      }
  
      def toXML(__obj: Address, __namespace: String, __elementLabel: String, __scope: scala.xml.NamespaceBinding): scala.xml.NodeSeq = {
        val prefix = __scope.getPrefix(__namespace)
        var attribute: scala.xml.MetaData  = scala.xml.Null
    
        scala.xml.Elem(prefix, __elementLabel,
          attribute, __scope,
          Seq.concat(scala.xml.Elem(prefix, "name", scala.xml.Null, __scope, scala.xml.Text(__obj.name.toString)),
            scala.xml.Elem(prefix, "street", scala.xml.Null, __scope, scala.xml.Text(__obj.street.toString)),
            scala.xml.Elem(prefix, "city", scala.xml.Null, __scope, scala.xml.Text(__obj.city.toString))): _*)
      }
    }


Bug Reporting
-------------

You can send bug reports to [Issues](http://github.com/eed3si9n/scalaxb/issues),
send me a [tweet to @eed3si9n](http://twitter.com/eed3si9n), or email.

Licensing
---------

It's the MIT License. See the file called LICENSE.
     
Contacts
--------

- eed3si9n at gmail dot com
- [@eed3si9n](http://twitter.com/eed3si9n)
