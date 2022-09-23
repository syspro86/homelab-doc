# Swing codes

## JTable

### [table clear](https://stackoverflow.com/questions/4577792/how-to-clear-jtable)

```
JTable table;
…
DefaultTableModel model = (DefaultTableModel) table.getModel();
model.setRowCount(0);
```

### [table sorter](https://stackoverflow.com/questions/28823670/how-to-sort-jtable-in-shortest-way)

```
JTable table = new JTable(tableModel);
TableRowSorter<TableModel> sorter = new TableRowSorter<TableModel>(table.getModel());
table.setRowSorter(sorter);

List<RowSorter.SortKey> sortKeys = new ArrayList<>(25);
sortKeys.add(new RowSorter.SortKey(4, SortOrder.ASCENDING));
sortKeys.add(new RowSorter.SortKey(0, SortOrder.ASCENDING));
sorter.setSortKeys(sortKeys);
```