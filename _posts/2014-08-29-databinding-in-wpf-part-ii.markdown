---
author: Arun Raj
comments: true
date: 2014-08-29 06:03:48+00:00
layout: post
slug: databinding-in-wpf-part-ii
title: 'DataBinding In WPF : Part II'
wordpress_id: 52
---

Now lets see the remaining parts of DataBinding


  * Converters in Data Binding

	
  * Multi Binding

	
  * Validation

	
  * Binding to Non-DP

	
  * Binding to Collections

	
  * Datatemplates and Data triggers

	
  * DataTemplate Selectors




**Converters In DataBinding**




In this part we'll see how value converters are used in data binding and we'll create a bool to visibility converter to better understand the concept. In the previous data binding scenarios we've directly bound the value of source to target. At times it'd be necessary to bind relative values to the target and provide a conversion from one format to another while the actual binding process. For such purposes Converters can be used. They provide means to assign relative values in databinding. Any converter class in databinding inherits from an Interface called IValueConverter. The class must contain two functions namely






	
  * `object Convert(object value, Type targetType, object parameter, CultureInfo culture)  and`

	
  * `object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)`




These functions provide means to convert data from one object to another while binding takes place.




Now suppose we have a Checkbox and we want to show something say a listbox while the checkbox is checked and remove the listbox while the checkbox is cleared. Lets see how this is done by implementing  bool to Visibility value converter. As for the example I'm going to use XAML and C# to implement this.




in c# create a class that inherits from IValueConverter and implement the convert functions. Within these functions use the logic required to implement the conversion.




```
class boolVis:IValueConverter
{
	public object Convert(object t,Type type, object parameter, CultureInfo culture)
	{
		return (t is bool && (bool)t) ? Visibility.Visible : Visibility.Hidden;
	}
	public object ConvertBack(object t, Type type, object parameter, CultureInfo culture)
	{
		return (t is Visibility && (Visibility)t == Visibility.Visible);
	}
}
```


In xaml refer to the namespace where you've written the code above and give it a name (in this example I gave it the name local). Now create an instance of the converter in the resources


` 
<local:boolVis x:Key="MyConverter"/>
`

Now I can use MyConverter in databinding as


```
<StackPanel>
<CheckBox Name="Mycheck" Content="Show the List"/>
<ListBox Background="Red" Height="70" Visibility="{Binding IsChecked, ElementName=Mycheck, Converter={StaticResource MyConverter}}" />
</Stackpanel>
```




I've assigned a background color to show the presence of the listbox in the UI.




**Multi-Binding**




Binding of multiple elements is done in WPF by the use of a MultiValue Converter. That is at the target property we use an instance of predefined MultiValue converter to bind more than one properties two a single target property. Suppose we have two text boxes that have their contents binded to a single textblock. To achieve this we first have to define a converter that is derived from `IMultiValueConverter` an implement its two functions``





`object Convert(object[] values, Type targetType, object parameter, CultureInfo culture);  and
object[] ConvertBack(object value, Type[] targetTypes, object parameter, CultureInfo culture);`




as you can see the Convert function accepts an array of value objects and returns a single value that is assigned to the target property. Similarly the ConvertBack accepts an object and returns the object array that get assigned to the source properties.




To accomplish our goal we first need to create the multivalue converter class. An example is given below





```
 class MultiBinderClass : IMultiValueConverter
        {
            public object Convert(object[] values, Type targetType, object parameter, CultureInfo culture)
            {
                return (string.Format(values[0] + " " + values[1]));
            }
            public object[] ConvertBack(object value, Type[] targetTypes, object parameter, CultureInfo culture)
            {
                throw new NotImplementedException();
            }
        }
```




Now in XAML we refer to the namespace and create an instance of the class. My namespace reference was `app` so I can create an instance of the class as





 `<app:MultiBinderClass x:Key="MultiValueConverter"/>`




If we have two text boxes as





```
 <TextBox x:Name="txtbox"/>
<TextBox x:Name="txtbox2"/>
```




Now I can bind the textblock as





```  <TextBlock>
            <TextBlock.Text>
                <MultiBinding Converter="{StaticResource MultiValueConverter}">
                    <Binding ElementName="txtbox" Path="Text"/>
                    <Binding ElementName="txtbox2" Path="Text"/>
                </MultiBinding>
            </TextBlock.Text>
        </TextBlock>
```




This would bind the two textbox contents to the textblock text property. Obviously we don't need a ConvertBack function.




**Validation in DataBinding**




Validation is used to provide information to the user about the type and format of the data that a field can have. It enforces some rules while source is being updated. Validation rules are processed when target to source binding takes place. Suppose a text box is used to enter the width of an interactive rectangle. Characters can also be entered within that text box, but this wouldn't be a meaningful value for the text box. Lets see how validation rules  can be used to provide a feedback to the user. Consider the XAML code below





```
<StackPanel Width="400">
            <Rectangle x:Name="RedRect" Fill="Red" Height="150" Width=" 150" />
            <TextBox>
                <TextBox.Text>
                    <Binding ElementName="RedRect" Path="Width" UpdateSourceTrigger="PropertyChanged">
                        <Binding.ValidationRules>
                            <ExceptionValidationRule/>
                        </Binding.ValidationRules>
                    </Binding>
                </TextBox.Text>
            </TextBox>
</StackPanel>
```


As a character is entered within the text box you can see a red border around the text box indicating that the value is invalid while binding. The `ExceptionValidationRule` can also be replaced with custom rules for validation. Lets see how to write a custom validation rule.




First we need to create a class that derives from ValidationRule. The derived class should override a method called Validate. The validate method returns a ValidationResult object after performing required validation logic.





`  public class WidthValidationRule : ValidationRule
    {
        public override ValidationResult Validate(object value, CultureInfo cultureInfo)
        {
            int result;
            if (Int32.TryParse((string)value, out result))
            {
                return new ValidationResult(true, null);
            }
            else
                return new ValidationResult(false, null);
        }
    }
    `




In XAML refer to the namespace containing the above class (given as local in this case) and then at ValidationRules of binding give the class name as

```
<Binding.ValidationRules>
	<local:WidthValidationRule/>
</Binding.ValidationRules>
```




**Binding to Non -DP s**




So far we've been binding data between UI elements. Lets see how to bind a dependency property to a CLR property. Since dependency properties have their own measure and arrange overrides whenever their value changes the element to which the property is bound to automatically gets updated. But this is not the scenario when we are binding a CLR property to a dependency property. In such cases when the value of a CLR property changes we need to convey to the UI that their values have been changed so that changes be reflected in the UI as well. Consider an example where you'd want to create a property that changes its value to a random value between 1-100 on a button-click. The class for the property should be defined as





```
public class Disp : INotifyPropertyChanged
    {
        Random rnd = new Random();
        private int randomvar;
        public int RandomVar
        {
            get { return randomvar; }
            set
            {
                randomvar = value;
                RaisePropertyChanged("RandomVar");
            }
        }
        public event PropertyChangedEventHandler PropertyChanged;
        private void RaisePropertyChanged(string propName)
        {
            if (PropertyChanged != null)
                PropertyChanged(this, new PropertyChangedEventArgs(propName));
        }
        public Disp()
        {
            Update();
        }
        public void Update()
        {
            RandomVar = rnd.Next(100);
        }
    }
```


And in the mouse click event we can call the Update() to update the value of the  RandomVar property.

**Binding to Collections**

Binding to collections is quite similar to binding to Non-Dps. Here we need to implement the PropertyChangedEvent for both the individual elements of the collection and the collection itself. This is to inform the UI that the number of elements in the collection has changed and need to be reflected in the UI. That is each element in the collection should be a property that fires the PropertyChangedEvent whenever its value gets changed. Consider a students collection bound to a listbox in XAML. Student element class should implement the INotifyPropertyChanged as


```
public class Student : INotifyPropertyChanged
    {
        private string _name;
        private int _age;
        public string Name
        {
            get
            {
                return _name.ToString();
            }
            set
            {
                _name = value;
                RaiseEventChanged("Name");
            }
        }
        public int Age
        {
            get
            {
                return _age;
            }
            set
            {
                _age = value;
                RaiseEventChanged("Age");
            }
        }
        public event PropertyChangedEventHandler PropertyChanged;
        private void RaiseEventChanged(string propName)
        {
            if (PropertyChanged != null)
            {
                PropertyChanged(this, new PropertyChangedEventArgs(propName));
            }
        }
        public override string ToString()
        {
            return _name.ToString();
        }
	}
 ```


We'll talk about the ToStringOverride() later in this topic. For now the above code declares two properties defined in a class that implements the INotifyPropertyChanged interface and the PropertyChangedEvent. Now lets see how collection class is defined. A collection class needs to implement INotifyCollectionChanged interface or any other class which inherits the INotifyCollectionChanged. For now lets consider the ObservableCollection<> class which inherits from INotifyCollectionChanged. The following code implements such a collection and adds some elements to the list.


```
class Students: ObservableCollection<Student>
{
	static int i;
	public Students()
	{
		Update();
	}
	public void Update()
	{
		for (i = 0; i < 10; i++)
		{
			Student s = new Student();
			s.Age = i * 5;
			s.Name = "Student_" + i.ToString();
			this.Add(s);
		}
	}
}

```


And on the button click we can give


`
private void Button_Click(object sender, RoutedEventArgs e)
{
	(this.DataContext as Students).Update();
}
`


But make sure that you've set the data context to a new instance of the Students class in the Constructor.


`this.DataContext = new Students();`


in the constructor should do the trick.

**Datatemplates and DataTriggers**

Data Templates and Data Triggers are used to style data based on its contents and type. Consider the same student class as of above. The XAML for such a listbox would be as     <ListBox Height=" 200" Width=" 200" ItemsSource="{Binding}"></ListBox>

Now if we want to template the Listbox we can use data templates. Data templates allow templating of controls and provide a better user experience. To give a background to the listbox items we can code as


```
  <DataTemplate DataType="{x:Type local:Student}">
      <Border Name="disco" BorderThickness="2" BorderBrush="Black" Background="Red"Padding="10">
          <TextBlock Text="{Binding Path=Name}" />
      </Border>
  </DataTemplate>
```


This allows templating of the objects of the class Student. We can also template an element based on its content. For that we use data triggers. The following code implements data triggers to the above code and changes the background property of elements with value "Student_3" to yellow.


```
    <DataTemplate DataType="{x:Type local:Student}">
            <Border Name="disco" BorderThickness="2" BorderBrush="Black" Background="Red"Padding="10">
                <TextBlock Text="{Binding Path=Name}" />
            </Border>
        <DataTemplate.Triggers>
                <DataTrigger Binding="{Binding Name}" Value="Student_3">
                    <Setter TargetName="disco" Property="Background" Value="Yellow" />
                </DataTrigger>
            </DataTemplate.Triggers>
        </DataTemplate>
```


**Datatemplate Selectors**

Data template selectors allow switching of data template based on the data content of the object. Consider a case when you want yellow background for integer values higher than a particular value and blue background the rest of the values in your listbox. For such situations we need to create a DataTemplateSelector class inherits from `DataTemplateSelectorClass`. It should also implement a function called ` DataTemplate SelectTemplate(object item, DependencyObject container)` and should return a datatemplate which may be defined in the some resource dictionary. For example


` return Window.FindResource("DataTemplateName") as DataTemplate;`


In XAML define the data templates in the any of resources dictionary in scope, create an instance of the class as we've discussed above and give that as the datatemplate in Listbox template property.
