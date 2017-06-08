---
title: Data Grouping
page_title: Data Grouping | RadListView for ASP.NET AJAX Documentation
description: Data Grouping
slug: listview/fuctionality/grouping/data-grouping
tags: data,grouping
published: True
position: 1
---

# Data Grouping

**RadListView** offers **data grouping** as of Q3 2013. You can group the data source of the control according to one or more specified data fields. The operation is performed on the data source of the control.

In order to define the layout for each group in the control, you should set separate **data group templates** in the **`DataGroupTemplate` tag** under each **`ListViewDataGroup`**.

You can access the **calculated aggregates** according to a predefined function.

In this article:

* [Features](#features)
* [Nested Groups](#nested-groups)
* [Server-side API](#server-side-api)
* [Limitations](#limitations)

## Features

You can take advantage of the following features of the DataGrouping functionality listed below:

* Collection of data groups in the markup

* Data field to group by for each data group

* Optional aggregates by field and aggregate function

* Template for each data group

* Nesting and hierarchical structures of the data groups and data items of the RadListView

* Group the data source using optimized LINQ queries when possible

* Server-side event for the purpose of custom calculation of the aggregates

Using the code below you will enable the RadListView DataGrouping feature following the listed functionalities.

````ASP.NET
<telerik:RadListView ID="RadListView1" runat="server" ItemPlaceholderID="DataGroupPlaceHolder3"
    InsertItemPosition="AfterDataGroups" DataSourceID="SqlDataSource1" AllowMultiFieldSorting="True"
    AllowPaging="True" GroupAggregatesScope="AllItems" DataKeyNames="Classification">
    <LayoutTemplate>
        <asp:Panel ID="DataGroupPlaceHolder1" runat="server"></asp:Panel>
    </LayoutTemplate>
    <ItemTemplate>
        <br />
        <div class="rlvI">
            <div class="category model">
                Model:
                <%#Eval("Model")%>
            </div>
            <div class="category">
                Classification:
                <%#Eval("Classification")%>
            </div>
            <div class="category">
                Year:<%#Eval("Year")%></div>
            <div class="category">
                Fuel Type:
                <%#Eval("Fuel")%>
            </div>
            <div class="category">
                Booking Price:
                <%#Eval("Price")%>
            </div>
            <div style="clear: both">
            </div>
        </div>
    </ItemTemplate>
    <%--Setting the DataGroups in the DataGroups tag--%>
    <DataGroups>
    <%--In each ListViewDataGroup GroupdFiled should be set.--%>
        <telerik:ListViewDataGroup GroupField="BrandName" DataGroupPlaceholderID="DataGroupPlaceHolder1"
            SortOrder="Ascending">
            <DataGroupTemplate>
                <asp:Panel runat="server" ID="Panel3" GroupingText='<%# (Container as RadListViewDataGroupItem).DataGroupKey %>'>
                    <asp:PlaceHolder runat="server" ID="DataGroupPlaceHolder2"></asp:PlaceHolder>
                    <asp:Label runat="server" ID="Label39" Text='<%# "Lower booking price: " + (Container as RadListViewDataGroupItem).AggregatesValues["Price"].ToString() %>'>
                    </asp:Label>
                </asp:Panel>
            </DataGroupTemplate>
           <%-- Set the aggregate by specific DataField in the GroupAggregate tags--%>
            <GroupAggregates>
                <telerik:ListViewDataGroupAggregate Aggregate="Min" DataField="Price" />
            </GroupAggregates>
        </telerik:ListViewDataGroup>
             <telerik:ListViewDataGroup GroupField="Classification" DataGroupPlaceholderID="DataGroupPlaceHolder2">
            <DataGroupTemplate>
                <asp:Panel runat="server" ID="Panel2" GroupingText='<%# (Container as RadListViewDataGroupItem).DataGroupKey %>'>
                    <asp:Panel runat="server" ID="DataGroupPlaceHolder3">
                    </asp:Panel>
                </asp:Panel>
            </DataGroupTemplate>
        </telerik:ListViewDataGroup>
    </DataGroups>
</telerik:RadListView>
<asp:SqlDataSource ID="SqlDataSource2" runat="server" ConnectionString="<%$ ConnectionStrings:TelerikConnectionString %>"
    SelectCommand="SELECT [BrandName], [Model], [Classification], [Year], [Fuel], [Price] FROM [Cars]"/>
````



Using the aggregate function on a specific aggregate field, you can set aggregates for specific groups. The aggregate value can be accessed using **AggregatesValues** property.

````ASP.NET
<telerik:ListViewDataGroup GroupField="BrandName" DataGroupPlaceholderID="DataGroupPlaceHolder1"
    SortOrder="Ascending">
    <DataGroupTemplate>
        <asp:Panel runat="server" ID="Panel1" GroupingText='<%# (Container as RadListViewDataGroupItem).DataGroupKey %>'>
            <asp:PlaceHolder runat="server" ID="PlaceHolder1"></asp:PlaceHolder>
            <asp:Label runat="server" ID="Label1" Text='<%# "Lower booking price: " + (Container as RadListViewDataGroupItem).AggregatesValues["Price"].ToString() %>'>
            </asp:Label>
        </asp:Panel>
    </DataGroupTemplate>
   <%-- Set the aggregate by specific DataField in the GroupAggregate tags--%>
    <GroupAggregates>
        <telerik:ListViewDataGroupAggregate Aggregate="Min" DataField="Price" />
    </GroupAggregates>
</telerik:ListViewDataGroup>
````



## Nested groups

You can achieve grouping in several levels by nesting the **ListViewDataGroups**.

Note that the **DataGroupPlaceHolderID** of each nested group should be set to be the PlaceHolder of its direct parent group. 

Also, the **ItemPlaceholderID** of the RadListView control should be set to the container's ID of the innermost group.

The example bellow presents two levels of grouping

````ASP.NET
<telerik:RadListView ID="RadListView1" runat="server" ItemPlaceholderID="DataGroupPlaceHolder3"
 InsertItemPosition="AfterDataGroups" DataSourceID="SqlDataSource1" AllowMultiFieldSorting="True"
 AllowPaging="True" GroupAggregatesScope="AllItems" DataKeyNames="Classification">
    <LayoutTemplate>
        <asp:Panel ID="DataGroupPlaceHolder1" runat="server"></asp:Panel>
    </LayoutTemplate>
    <ItemTemplate></ItemTemplate>
   <DataGroups>
     <telerik:ListViewDataGroup GroupField="BrandName" DataGroupPlaceholderID="DataGroupPlaceHolder1"
         SortOrder="Ascending">
         <DataGroupTemplate>
             <asp:Panel runat="server" ID="Panel4" GroupingText='<%# (Container as RadListViewDataGroupItem).DataGroupKey %>'>
                 <asp:PlaceHolder runat="server"  ID="DataGroupPlaceHolder2"></asp:PlaceHolder>
             </asp:Panel>
         </DataGroupTemplate>
        <%-- Set the aggregate by specific DataField in the GroupAggregate tags--%>
     </telerik:ListViewDataGroup>
          <telerik:ListViewDataGroup GroupField="Classification" DataGroupPlaceholderID="DataGroupPlaceHolder2">
         <DataGroupTemplate>
             <asp:Panel runat="server" ID="Panel5" GroupingText='<%# (Container as RadListViewDataGroupItem).DataGroupKey %>'>
                 <asp:Panel runat="server" ID="DataGroupPlaceHolder3">
                 </asp:Panel>

             </asp:Panel>
         </DataGroupTemplate>
     </telerik:ListViewDataGroup>
 </DataGroups>
</telerik:RadListView>
<asp:SqlDataSource ID="SqlDataSource1" runat="server" ConnectionString="<%$ ConnectionStrings:TelerikConnectionString %>"
    SelectCommand="SELECT [BrandName], [Model], [Classification], [Year], [Fuel], [Price] FROM [Cars]" />
````



## Server-Side API

Several new properties have been exposed for the DataGrouping functionality:


|  **Property**  |  **Description**  |
| ------ | ------ |
| **GroupAggregatesScope** |Sets the way the aggregates for specific group is calculated–for the specific page or for all items. The default value is "AllItems"|
| **InsertItemPostion** (extended enumeration)|Sets the position of the insert item. The new enumeration includes **"BeforeDataGroups"** and **"AfterDataGroups"** |
| **DataGroupKey** |Holds the data key name of the current data group|

**Events:**


|  **Events**  |  **Description**  |
| ------ | ------ |
| **OnCustomAggregate** |Fired when the **aggregate type** is **“Custom.”** User can specify custom calculated value because of the aggregate.|

## Limitations

* **Unsupported data sources/bindings**

  * XMLDataSource

  * SQLDataReader

  * Simple databinding

  * Client-side binding

* **Unsupported features**

  * Custom paging/sorting/filtering

  * Using simple (UI) grouping results in the "ItemPlaceHolder not specified" error

## See Also

* [Live Demo: Data Grouping](http://demos.telerik.com/aspnet-ajax/listview/examples/datagrouping/defaultcs.aspx)

* [Simple (UI) Grouping]({% slug listview/fuctionality/grouping/overview %})