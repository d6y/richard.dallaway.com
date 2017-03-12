---
layout: post
comments: true
permalink: /:title.html
title: 'JasperReports, JFreeChart, JExcelAPI'
author: Richard
date: 2006/02/22
alias: /jasperreports-jfreechart-jexcelapi
tags:
---

I had some reporting to add to an application last year, and rather than
code something from scratch, I decided to dig around for a package to
give us something we could grow with—something to give us a framework,
that we could use to make more sophisticated reports when the time came
for them.

[JasperReports][] looks good: you specify the data going into the
report, you declaratively set up the report in XML, and at the last
minute you decide the format you want for output: HTML, Excel, PDF.
There's even built-in support for it in WebWork. "Over 1 million"
downloads can't be wrong.

It works, but it hurts. In fairness, if you look at the [gallery][] of
reports, and you need to produce something just like that, I suspect
JasperReports is worth looking at. Alas, I found it takes a huge amount
of time to get up to speed with JasperReports just to find out if it can
do what you want to do. If you want to dig into JasperReports: download
the distribution and look in the `demo/samples/` folder—there are more
things the product can do than shown on the web site. If any of those
samples match what you want, try to hack the `.jrxml` to your liking.

Then go and look at [BIRT][] and compare.

In the end what we really needed was more emphasis on the graphical
representation of the data, with more web-like reporting. JasperReports
is all about "page oriented" output which is a tough problem to solve,
without all my additional needs.

So we rolled our own code using [JFreeCharts][] in a fraction of the
time it took to learn to use JasperReports. JFreeCharts is excellent,
but I'd recommend getting the commercial [documentation][].

For Excel output, try [JExcelAPI][]:

	WritableWorkbook book = Workbook.createWorkbook( outputStream );
	WritableSheet sheet = book.createSheet(title, 0);
	
	// Set up the column headers: note the params are (column, row)
	sheet.addCell(new Label(0, 0, "Date")); // cell A1 
	sheet.addCell(new Label(1, 0, "Count")); // cell A2
	
	// Format to use for values:
	WritableCellFormat intFormat = new WritableCellFormat(NumberFormats.INTEGER);
	WritableCellFormat dateFormat = new WritableCellFormat(new DateFormat("yyyy/MM/dd"));
	
	for (Data d : data)
	{
	 sheet.addCell(new DateTime(0, d.getRow(), d.getDate(), dateFormat));
	 sheet.addCell(new Number(1, d.getRow(), d.getCount(), intFormat));
	}
	
	book.write();
	book.close();


I should also add that the idea of writing one report and having it
output in different formats isn't really how things worked for us in
practice. What we wanted was one set of details shown to the user, but
for download we wanted a slightly different report (more detail, less
graphics).



  [JasperReports]: http://jasperreports.sourceforge.net/
  [gallery]: http://jasperreports.sourceforge.net/samples/index.html
  [BIRT]: http://www.eclipse.org/birt/
  [JFreeCharts]: http://www.jfree.org/jfreechart/index.php
  [documentation]: http://www.object-refinery.com/jfreechart/guide.html
  [JExcelAPI]: http://www.andykhan.com/jexcelapi/
