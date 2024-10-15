# pag 1 
Oncharge pag: Set( varFrom;0);;
Select(btnSearch);;

lblResultsPerPage = text: "Results per page" 

btnPrev
Set(varFrom;varFrom-drpPaginationSize.Selected.Value);;Select(btnSearch)
visible:varFrom<>0

btnSearch
onselect: Set(
    varSearchResults;
    SearchinDocuments.Run(
        Coalesce(
            txtSearch.Text;
            "*"
        );
        drpPaginationSize.Selected.Value;
        varFrom
    )
);;
Set(
    varTotalCount;
    varSearchResults.totalrows
);;
Set(
    varTotalCount;
    varSearchResults.rowcount
);;


drpPaginationSize
items: [5;10;20;50]
Defaultselectitems: [10]
Oncharge: Set(varFrom;0);;Select(btnSearch)

btnNext
Onselect: Set(varFrom;varFrom+drpPaginationSize.Selected.Value);;Select(btnSearch)
Visible: Value(varSearchResults.totalrows) > varFrom + drpPaginationSize.Selected.Value

lblTotalResults
text: "Total Search Results: " & varSearchResults.totalrows

txtSearch
Onchange: Set(
    varFrom;
    0
);;
Select(btnSearch)

galSearchResults
items: Table(ParseJSON( varSearchResults.searchresults))
image: Coalesce( Text(LookUp(Table(ThisItem.Value.Cells);ThisRecord.Value.Key = "PictureThumbnailURL").Value.Value);SampleImage)

lblFileName
text: LookUp(
    Table(ThisItem.Value.Cells);
    ThisRecord.Value.Key = "FileName"
).Value.Value

lblID
text: "ID 🏷️ " & LookUp(Table(ThisItem.Value.Cells);ThisRecord.Value.Key = "listitemid").Value.Value

lblFileSize
text: "Size 📂 " &  Text( LookUp(Table(ThisItem.Value.Cells);ThisRecord.Value.Key = "size").Value.Value/1000000;"0.00") & " MB"

lblCreatedBy|
"Created By ➡️  "  & LookUp(Table(ThisItem.Value.Cells);ThisRecord.Value.Key = "Author").Value.Value

lblFileViews
text: "Views 👀 " &  Coalesce( Text(LookUp(Table(ThisItem.Value.Cells);ThisRecord.Value.Key = "ViewsLifeTime").Value.Value); "0")

botaao : btnLaunchDoc
Onselect: If(
    Text(
        LookUp(
            Table(ThisItem.Value.Cells);
            ThisRecord.Value.Key = "FileType"
        ).Value.Value
    ) in [
        "docx";
        "pptx";
        "xlsx";
        "csv"
    ];
    Launch(
        LookUp(
            Table(ThisItem.Value.Cells);
            ThisRecord.Value.Key = "ServerRedirectedURL"
        ).Value.Value
    );
    Launch(
        LookUp(
            Table(ThisItem.Value.Cells);
            ThisRecord.Value.Key = "DocumentLink"
        ).Value.Value
    )
)
/*If(
    Text(
        LookUp(
            Table(ThisItem.Value.Cells),
            ThisRecord.Value.Key = "FileType"
        ).Value.Value
    ) in [
        "png",
        "jpg",
        "mp4"
    ],
    Launch(
        Index(
            Split(
                LookUp(
                    Table(ThisItem.Value.Cells),
                    ThisRecord.Value.Key = "ParentLink"
                ).Value.Value,
                "Forms/AllItems.aspx"
            ),
            1
        ).Value & Text(
            LookUp(
                Table(ThisItem.Value.Cells),
                ThisRecord.Value.Key = "title"
            ).Value.Value
        ) & "." & Text(
            LookUp(
                Table(ThisItem.Value.Cells),
                ThisRecord.Value.Key = "FileType"
            ).Value.Value
        )
    ),
    If(
        Text(
            LookUp(
                Table(ThisItem.Value.Cells),
                ThisRecord.Value.Key = "FileType"
            ).Value.Value
        ) in [
            "docx",
            "pptx",
            "xls","csv"
        ],
         Launch(
            LookUp(
                Table(ThisItem.Value.Cells),
                ThisRecord.Value.Key = "ServerRedirectedURL"
            ).Value.Value
        ),
        Launch(
            LookUp(
                Table(ThisItem.Value.Cells),
                ThisRecord.Value.Key = "DocumentLink"
            ).Value.Value
        )
    )
)*/

text: "📁 " & With(
    {
        x: First(
            Split(
                Index(
                    Split(
                        LookUp(
                            Table(ThisItem.Value.Cells);
                            ThisRecord.Value.Key = "path"
                        ).Value.Value;
                        "/sites/RDTech/"
                    );
                    2
                ).Value;
                "/"
            )
        ).Value
    };
    If(
        IsError(x);
        "Home";
        x
    )
)
