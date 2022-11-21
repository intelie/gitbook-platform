---
description: >-
  A heatmap (or heat map) is a graphical representation of data where the
  individual values contained in a matrix are represented as color [Wikipedia].
---

# Heatmap

## Pipes query

The heat map pipes query should output events with a “point” property should be a seq with 3 values, corresponding to the x, y and value of each cell, correspondingly.&#x20;

![Figure 1: Heatmap example. A 10x10 matrix with values in each cell ranging from 0 to 1000.](https://lh4.googleusercontent.com/v71Y9l6NjUjKa3yqggCn0tDiZwAx-TCU1nApuyT-sW8GZ1HhHwpZTXFdx46UWNLgIbMX5taw19kLoNAaouJAGh9wurMrX3wAfw2pjlhmqXYaY2ne4o-VSPWwn4GPvxx2wSv35Mfh)

For example, the following Pipes query (with span ts 1 to ts 10) will generate a hea tmap similar to Figure 1, outputting events similar to Figure 2:\


```
=> random(), timestamp() as t every ms
=> @for range(10) |> (t, _ +1, random() * 1000//1):seq() as point
```

![Figure 2: Events containing the values of the cells in the first row, columns one to ten.](https://lh6.googleusercontent.com/-y0esjdhz5ElwkAN92QzT-iPiWooYtAp2LuZaowNhhYc7N8VUX9FmWpvYdtfS\_vTigTEhl65Z4cpcmRqnopo3gd2Ppo89FyfsWPPJaUVs57bEIr39nz4z5EaMDGMDE7ZmC9HTGis)

## Cell settings

With the default cell settings (Figure 3), as long as there is a point for every one x and y values, the heat map will render with no spacing between the cells (see Figure 1). You can change this setting to make cells span over some range. Otherwise, there will be gaps between the cells.

![Figure 3: Default settings assume one cell for every integer in each of the X and Y axes.](https://lh3.googleusercontent.com/8FQkGok5Y1tJ-ZvfMHmZq91JHSdUx8ebV562JgTqUmhA-lWyKvFpuZslY7jnNSEYV3EmXlTk7llDrIL2jYQSpFM-RqfDMCPWfPqWvPmjtDP0QvG9G1kNoLJBUImV\_0cM6w6wRGlv)

Should we edit the previous query and multiply every y value for 2 we would have a space between every cell in the y axis, such as in Figure 4

```
=> random(), timestamp() as t every ms
=> @for range(10) |> (t, (_+1)*2, random() * 1000//1):seq() as point
```

\


![Figure 4: Heat map with the default cell settings and one cell every 2 y values](https://lh4.googleusercontent.com/fyLsRsTotWbtM3OK7LWcIi-Qu1aKROsS2V9QWpOFAzdNIMYb9nxw5\_RKsUlZLx6t4b37WFDXaWBJw\_eCS99lzedj3gxMEtKomPSGAcJwpiykT3jWajC8fP7OEi\_SvAUM4b\_Pl8sA)

Similarly, should we also multiply every x value for 3 we would have a space the size of two cells between every cell in the x axis, such as in Figure 5

```
=> random(), timestamp() as t every ms
=> @for range(10) |> (t*3, (_+1)*2, random() * 1000//1):seq() as point
```

\


![Figure 5: Heat map with the default cell settings and one cell every 3 x values and every 2 y values](https://lh4.googleusercontent.com/gt\_etl1G3xsIJ3v-d8cpB5l6fPi8IC6YbFqg\_OXhFKpcKCm1zbzD3DzWHQlGf0jMckqmz8q8dcvQ1Xbbi2EVMiNn3d18uxSTQhC5IfARgT24-Who7fpgHz08yCVbxv9uC2odwRy-)

If the spacing between cells is not desired, one can simply set column width to 3 and the row height to 2, setting depicted in Figure 6. In this case, the chart has one point every 3 X axis values so the column width is set to 3. It has one point every 2 y values so the row height is set to 2. With this setting, there will be no spaces between cells.

\


![](https://lh6.googleusercontent.com/F56B1UcEodXuVgWslQm6QtnxXsJnSNulqYruB8Xkk8ZM\_qsuXIkjGkmqTOaw4JETf2G029QjDiRS1YRZvm4gZQzUYsJXLCSm5dNoUeym9Z8tKfJpVOmphEHTkUyLbMQoU2Uiyko1)

![Figure 6: Cell settings to match data frequency](https://lh3.googleusercontent.com/c2N1LSjNcOyCx\_ZpyiAn0rjtmairasujH6QtY2xmEzlHZL6CRAFPaAYRb3duMhDQ41tfdWaPFDM1vjl0OLXokQx4fqJmbn\_TOn\_oKw4BNPW5lqKGgIOhqJZDLAmS4j71PButgtZe)

## Cell value overwrite

Should new values arrive for an existing cell, that cell will show its most recent value

The following query will output a chart similar to the one in Figure 7:

```
=> random() every seconds
=> @for range(10) |> (random()*10//1, _, (random() * 1000)//1):seq() as point
```

![Figure 7: Chart with cell values being overwritten](https://lh3.googleusercontent.com/hJlg2ktH3fqU6W21YOSklBvCUtIboc0iEtmgGPoizM-GmsO74v\_CdB-zec1\_YA0ZxS9m64JrslCGA-smPEl1mnwDlTqJMAzL1RXIHlHq70PEF3NsdkThEoMeoq9HkbOv0XhHCfFQ)

## Troubleshooting

* Q:  Chart shows no axis and no data
* A:  Data is not in the right format. Each event should have a “point” property with a seq containing the x,y and value for each cell. Check the Pipes Query section above on how to format the data output for more details.

![Figure 8: Troubleshooting - Chart shows no axis and no data](https://lh6.googleusercontent.com/61TodWyQzbX5pyFIZl4U8mjEX0kVHB-gA3pdYjahW-YhjBngiDvOVASSkIPyh2zsMV66eviYekTA20nXJh06DjTMJmf-J36ijiR7fR2Tn\_cjkvAXtGu7K3ukrnB97xf2sk9LcdSR)

* Q: I have a working query outputting valid points but my chart looks blank
* Q: Chart shows axis but no cells
* A: There is probably too much space between cells making the cell sizes smaller than a pixel. Increase the cell settings width or height to match the data frequency.  Read the cell settings above on how to set up the row height and column width to match the data frequency

![Figure 9: Troubleshooting - Heat map with data but without any visible cells](https://lh6.googleusercontent.com/O8nncvj9R99MICOEW9udZuhgVGV6d\_O45J6FitKGsdXRTDEM5iIQHmhcjvB74-eYlesPopGwU7a9LIjn2\_z3ycCZtSG7MMoDfN1Zeoxm5cYswDTFnWPHHJhq7u6yz687bnt9jxtv)

* Q: There are gaps between the cells
* A: The cell width or height is too small. Increase the cell settings values to match the data frequency. Read the cell settings above on how to set up the row height and column width to match the data frequency

![Figure 10: Troubleshooting - Chart shows spaces between cells](https://lh4.googleusercontent.com/gt\_etl1G3xsIJ3v-d8cpB5l6fPi8IC6YbFqg\_OXhFKpcKCm1zbzD3DzWHQlGf0jMckqmz8q8dcvQ1Xbbi2EVMiNn3d18uxSTQhC5IfARgT24-Who7fpgHz08yCVbxv9uC2odwRy-)

* Q: Cells have irregular size
* Q: Cells show over one another when I hover over them.
* A: Cell settings have width or height settings values greater than the frequency of data. Reduce the cell settings width or height to match the data frequency. Read the cell settings above on how to set up the row height and column width to match the data frequency

![Figure 11: Troubleshooting - Heatmap with cells with irregular sizes showing over other cells](https://lh5.googleusercontent.com/IY6y0m0JG6NwxM1CAaBzik6Pjoj4avmb\_uYn4TkiagwC1TJp5AVXUrmvtvTcE0sQ8naVMi9uSOFRD22ok77pWJMERDY7qFLLYAv2h0LWmNUu1NEdonjETqqDjcEloTZNIgL4MBlj)
