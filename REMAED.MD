```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;


using Newtonsoft.Json;

namespace API
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }


        //https://xxapi.cn/doc/ip
        string api_url = "https://v2.xxapi.cn/api/ip";

        //创建HttpClient实例
        HttpClient client = new HttpClient();


        private async void button1_Click(object sender, EventArgs e)
        {
            //GET形式发送请求到指定的URL，并获取响应
            HttpResponseMessage response = await client.GetAsync(api_url);


            //将响应的状态码显示在label1上
            label1.Text =  response.StatusCode.ToString().Trim();

            //这个也可以显示状态码，但我觉得直接显示状态码更好一些
            richTextBox2.Text = response.EnsureSuccessStatusCode().ToString();


            //从响应中读取内容，并将其显示在richTextBox1上
            string json = await response.Content.ReadAsStringAsync();  
            richTextBox1.Text = json;



            //使用JsonConvert.DeserializeObject方法将JSON字符串解析为动态对象

            //dynamic和 object 的区别：
            //object：虽然也能装任何数据，但在你调用它的方法时，编译器会检查你是不是写错了。
            //dynamic：编译器完全“闭嘴”，不管你怎么调用（比如 result.data.ip），它都认为是合法的，直到程序运行那一刻。如果运行时发现它根本没有 data 属性，程序才会报错。
            dynamic result = JsonConvert.DeserializeObject(json);

            //将解析后的对象转换为字符串，并显示在richTextBox2上
            richTextBox3.Text = result.ToString();


            //需要什么就直接从已经解析后的对象(result)中获取就好了
            textBox1.Text = result.data.ip;

        }

        private void button2_Click(object sender, EventArgs e)
        {
            richTextBox1.Clear();
            textBox1.Clear();
            richTextBox3.Clear();
            richTextBox2.Clear();
        }








    }
}
```