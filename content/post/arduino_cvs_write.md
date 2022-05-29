+++
title='Arduino读取cvs格式'
tags=[]
categories=[]
date="2022-05-29T18:36:22+08:00"
toc=true
draft=false
hiddenFromHomePage= false
+++


### 工具
```

int csvReadText(File *file, char *str, size_t size, char delim)
{
  uint8_t ch;
  int rtn;
  size_t n = 0;
  while (true)
  {
    // check for EOF
    if (!file->available())
    {
      rtn = 0;
      break;
    }
    if (file->read(&ch, 1) != 1)
    {
      // read error
      rtn = -1;
      break;
    }
    // Delete CR.
    if (ch == '\r')
    {
      continue;
    }
    if (ch == delim || ch == '\n')
    {
      rtn = ch;
      break;
    }
    if ((n + 1) >= size)
    {
      // string too long
      rtn = -2;
      n--;
      break;
    }
    str[n++] = ch;
  }
  str[n] = '\0';

  Log.traceln("readText %s", str);
  return rtn;
}
//------------------------------------------------------------------------------
int csvReadInt32(File *file, int32_t *num, char delim)
{
  char buf[20];
  char *ptr;
  int rtn = csvReadText(file, buf, sizeof(buf), delim);
  if (rtn < 0)
    return rtn;
  *num = strtol(buf, &ptr, 10);
  if (buf == ptr)
    return -3;
  while (isspace(*ptr))
    ptr++;
  return *ptr == 0 ? rtn : -4;
}
//------------------------------------------------------------------------------
int csvReadInt16(File *file, int16_t *num, char delim)
{
  int32_t tmp;
  int rtn = csvReadInt32(file, &tmp, delim);
  if (rtn < 0)
    return rtn;
  if (tmp < INT_MIN || tmp > INT_MAX)
    return -5;
  *num = tmp;
  return rtn;
}
//------------------------------------------------------------------------------
int csvReadUint32(File *file, uint32_t *num, char delim)
{
  char buf[20];
  char *ptr;
  int rtn = csvReadText(file, buf, sizeof(buf), delim);
  if (rtn < 0)
    return rtn;
  *num = strtoul(buf, &ptr, 10);
  if (buf == ptr)
    return -3;
  while (isspace(*ptr))
    ptr++;
  return *ptr == 0 ? rtn : -4;
}
//------------------------------------------------------------------------------
int csvReadUint16(File *file, uint16_t *num, char delim)
{
  uint32_t tmp;
  int rtn = csvReadUint32(file, &tmp, delim);
  if (rtn < 0)
    return rtn;
  if (tmp > UINT_MAX)
    return -5;
  *num = tmp;
  return rtn;
}
//------------------------------------------------------------------------------
int csvReadDouble(File *file, double *num, char delim)
{
  char buf[20];
  char *ptr;
  int rtn = csvReadText(file, buf, sizeof(buf), delim);
  if (rtn < 0)
    return rtn;
  *num = strtod(buf, &ptr);
  if (buf == ptr)
    return -3;
  while (isspace(*ptr))
    ptr++;
  return *ptr == 0 ? rtn : -4;
}
//------------------------------------------------------------------------------
int csvReadFloat(File *file, float *num, char delim)
{
  double tmp;
  int rtn = csvReadDouble(file, &tmp, delim);
  if (rtn < 0)
    return rtn;
  // could test for too large.
  *num = tmp;
  return rtn;
}
```

### 使用
read track.txt
32.11224,118.9098393,trackname
```
 
   File trackfile = SD.open("track.txt", FILE_READ);

       while (trackfile.available())
      {
         if (csvReadDouble(&trackfile, &TRACK_LAT, ',') != ',' || csvReadDouble(&trackfile, &TRACK_LNG, ',') != ',' || csvReadText(&trackfile, TRACKNAME, sizeof(TRACKNAME), ',') != '\n')
        {
           Log.traceln("track.txt read error");
         }

         Log.traceln("track.txt %D %D %s", TRACK_LAT, TRACK_LNG, TRACKNAME);
       }
      trackfile.close(); 
```