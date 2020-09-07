using System;
using System.Collections.Generic;//Allows lists to be used
using System.Linq;//For use of Linq operations
using System.Text;
using System.Threading.Tasks;
using System.IO;//Allows the access of outside source files
using System.Text.RegularExpressions; //For use of Regex operations

namespace TextSummeriser
{
    class Program
    {
        static void Main(string[] args)
        {
            
            char[] delimiterChars = { ' ', ',', ':', '[', ']', '(', ')', '-', '\r', '\n'};
            //Chars that will be removed by the checker
            char[] splitSentPoint = { '.' };
            //Char to split at a '.'
            string lineText;
            //String to store lines of text
            Console.WriteLine("Summarize Text Program please enter between 1 and 100 (100 is a completely un-summarised text)\r\n");

            List<string> textList = new List<string>();
            //Creates a new list for use

            using (StreamReader reader = new StreamReader("inFile.txt"))
            {
                //Stream Reader to read the users inputFile
                while ((lineText = reader.ReadLine()) != null)
                {
                    textList.Add(lineText);
                    //Adds input test to a List
                }

            }

            int wordCount = textList.Count();
            //finds and creates a list from the user input file.

            StreamWriter outFileWriter = new StreamWriter("outFile.txt");
            //StreamWriter to write file to an output.

            string splitText = string.Join(" ", textList.ToList());
            //Creates a string from a List

            string[] splitParts = splitText.Split(delimiterChars, StringSplitOptions.RemoveEmptyEntries);
            //removes the required chars from the string

            //Converts List in to a string and splits creating individual strings for each element in the list
            List<string> unremovedSplitList = new List<string>(splitParts);

            List<string> splitList = new List<string>(splitParts);
            //Takes the split strings and creates a List

            string splitSentence = string.Join(". ", textList.ToList());

            string[] sentParts = splitSentence.Split(splitSentPoint);            
            //Splits sentence at its split point
            List<string> splitSentList = new List<string>(sentParts);
            foreach (string s in splitSentList)
            {
                Console.WriteLine(s);
            }
            //Split Textfile in to Sentences

            List<string> inpStopList = new List<string>();
            using (StreamReader reader = new StreamReader("stopWords.txt"))
                //Uses StreamReader to find the stopwords document
            {
                string stopper;
                while ((stopper = reader.ReadLine()) != null)
                {
                    inpStopList.Add(stopper);
                    //Adds whole stopWords file to a list.
                }
            }
            //finds and creates a list from the stop file.
            
            string stopCsv = string.Join(" ", inpStopList.ToList());
            //Split stopWords in to a string splitting at WhiteSpace.

            string[] stopSpl = stopCsv.Split();
            //Converts List in to a string and splits creating individual strings for each element in the list

            List<string> stopList = new List<string>(stopSpl);
            //Creates the list for the stopwords.

            splitList.RemoveAll(word => stopList.Contains(word));
            //Removing stop words from the user provided txt file

            List<string> removedList = splitList.ToList();
            //Puts new output to a list

            Console.WriteLine("\r\nWord Freqences:");
            foreach (var f in removedList.GroupBy(i=>i).OrderByDescending(f=>f.Count()))
            {
                Console.WriteLine("{0}, {1}", f.Key, f.Count());
                //Displays words that have been ordered by frequency
            }

            Console.WriteLine("Please enter your summarization factor");
            int textWordCount = Convert.ToInt32(unremovedSplitList.Count);
            //Converts the count of the split list to an int
            int userSF = Convert.ToInt32(Console.ReadLine());
            //Converts Users input to an integer
            Console.WriteLine("Your Document will be Summerized to: " + userSF + "%");
           
            var mostFreq = removedList.GroupBy(i => i)
                .OrderByDescending(grp => grp
                .Count())
                .Select(grp => grp.Key)
                .ToList();
            //orders and counts the list of words with stopwords removed and puts to a List
            
            foreach (string s in mostFreq.ToList())
            {
                mostFreq.Add(s);
            }
                        
            List<string> unorderedSent = new List<string>();

            foreach(string s in splitSentList)
            {
                if (s.Contains(mostFreq.First()))
                //if the split sentence list contains the most frequent word...
                    mostFreq.Add(s);
                    unorderedSent.Add(s);
                //add output of the if to an unordered sentence list

            }

            Console.WriteLine("\r\nSummarized Text: \r\n");
            
            List<string> finalList = new List<string>();
            
            string mostComm = @"\b" + mostFreq.First() + @"\b";
            //creates a Variable for a Regex that stores the most frequent Current word

            int sf = 0;

            string finString = "";

            if (userSF == 100)
            {
                Console.WriteLine(splitSentence);
                outFileWriter.WriteLine(splitSentence);
            }
            //if 100% is chosen by the user output whole text file.
            else
            {
            //If user has not chosen 100%...
                while (sf < userSF) //While text list count is less than the user input
                {

                    var ordered = unorderedSent.OrderByDescending(o => Regex.Matches(o, mostComm).Count)
                       .First()
                       .ToList();
                    //Takes the most common word and compares it with the unordered sentence List


                    string completeFirst = string.Join("", ordered.ToList());
                    foreach (string check in unorderedSent.ToList())
                    {
                        if (check.Contains(mostFreq.First()))
                        {
                            //If first most common word is contained in the unordered sentence List
                            unorderedSent.Remove(check);
                            //Removes the Top sentence from the List
                        }
                    }

                    splitSentList.Remove(splitSentList.First());
                    //removes the first Sentence from the split sentence list
                    removedList.Remove(mostFreq.First());
                    //Removes the First/ Most common word from the removed list
                    finalList.Add(completeFirst);
                    //Adds final output to a List
                    unorderedSent.Remove(completeFirst);
                    //Removes Top matched from list so it cant be used again

                    finString = string.Join(".", finalList.ToList());
                    //Converts final list to a string
                    sf = ((finString.Split(' ').Count() * 100) / textWordCount);
                    //Calculation for Percentage
                }
            }
            finString.Count();
            outFileWriter.Write(finString + "\r\nTotal words of orginal file were: " + splitText.Count() + "\r\nText summarized to " + userSF + "%" + "\r\nSummarized to " + finString.Count()+ " word.");
            //Writes to the outFile final summarized text, total orginal word count, users SF factor and final count of words in the finished summarization
            outFileWriter.Close();
            //Closes the Writer to stop the writing in to the output doc.
            Console.WriteLine(finString + "\r\nTotal words of orginal file were: " + splitText.Count() + "\r\nText summarized to " + userSF + "%" + "\r\nSummarized to " + finString.Count() + " word.");
            //Displays final output in console

            Console.WriteLine("Press any key");
            Console.ReadKey();
        }
    }

}

//References
//Microsoft, 2015. How to: Parse Strings Using String.Split (C# Programming Guide). [online] 
//Available at: <https://msdn.microsoft.com/en-us/library/ms228388.aspx>[Accessed 25 October 2015]

//Dotnetperls, n.d. Convert List, String. [online] 
//Available at: <https://www.dotnetperls.com/convert-list-string>[Accessed 25 October 2016]

//Stackoverflow, 2015. Sign upSort or Order a List of strings containing a specific word with LINQ [online] 
//Available at: <http://stackoverflow.com/questions/28592350/sort-or-order-a-list-of-strings-containing-a-specific-word-with-linq>[Accessed 15 November 2016]

//Microsoft, 2007. C# User Input [online]
//Available at: <https://social.msdn.microsoft.com/Forums/en-US/a28fd047-6469-42bf-b79b-46112a4d7a9f/c-user-input?forum=csharpgeneral>[Accessed 20 November 2016]
