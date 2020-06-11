# Check If It Is a Straight-Line

You are given an array coordinates, coordinates[i] = [x, y], where [x, y] represents the coordinate of a point.
Check if these points make a straight line in the XY plane.

<a href="https://ibb.co/BtqSWpB"><img src="https://i.ibb.co/Vt2Pn1T/Straight-Line.jpg" alt="Straight-Line" border="0"></a>
<br>
```csharp

 public static bool CheckStraightLine(int[,] coordinates){
            List<Point> points = new List<Point>();

            int counter = 1;
            string x =string.Empty;
            string y = string.Empty;
            foreach (var item in coordinates)
            {
                if (counter % 2 != 0){
                    x = item.ToString();
                }
                if (counter % 2 == 0){
                    y = item.ToString();
                }

                if (!string.IsNullOrEmpty(x) && !string.IsNullOrEmpty(y)){
                    Point point = new Point(Convert.ToInt32(x),Convert.ToInt32(y));
                    points.Add(point);
                    x = string.Empty;
                    y = string.Empty;
                }
                counter++;
            }

            double slop = 0;
            List<double> slopes = new List<double>();
            foreach (Point pointMaster in points){
                foreach (Point pointDetail in points){
                    try{
                        slop = (pointMaster.Y - pointDetail.Y) / (pointMaster.X - pointDetail.X);
                        slopes.Add(slop);
                    }
                    catch (DivideByZeroException ex){
                        continue;
                    }
                }
            }
            bool retVal = false;
            IEnumerable sayis = slopes.Distinct();
            if (Count(sayis) == 1)
                retVal = true;
            return retVal;
        }

        public static int Count(IEnumerable source)
        {
            if (source == null){
                throw new ArgumentNullException("source");
            }

            // Optimization for ICollection implementations (e.g. arrays, ArrayList)
            ICollection collection = source as ICollection;
            if (collection != null){
                return collection.Count;
            }

            IEnumerator iterator = source.GetEnumerator();
            try{
                int count = 0;
                while (iterator.MoveNext()){
                    count++;
                }
                return count;
            }
            finally{
                IDisposable disposable = iterator as IDisposable;
                if (disposable != null){
                    disposable.Dispose();
                }
            }
        }
