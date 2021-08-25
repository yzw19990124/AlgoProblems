[36372985.zip](https://github.com/yzw19990124/AlgoProblems/files/7048389/36372985.zip)
# AlgoProblems
"""
Author: Yanzheng Wu
Date: April 28th, 2020
File Description: CS330 Program Excercise 03
BU ID: U89934368
"""
import sys

class UnexpectedBacktraceSymbol(Exception):
    """
    Temp dont do anything
    """
    pass

def display(arrs):
    """
    Helper function just for memoization chart,
    has deleted function call
    :param arrs: mem table
    :return: None
    """
    for arr in arrs:
        for e in arr:
            s = str(e)
            print(f"{s:4s}", end="")
        print()

def editDistance(s1, s2):
  """
  This function is for editing two lines from inputs,
  call backtrace to get edit matrix
  """
  rows, backtraces = costmatrix(s1, s2)
  edits = backtrace(backtraces)
  return rows[-1][-1], edits

def costmatrix(s1, s2):
  rows = []

  previous_row = range(len(s2) + 1)
  rows.append(list(previous_row))
  backtraces = [['m'] + ['+'] * len(s2)]

  for i, c1 in enumerate(s1):
    current_row = [i + 1]
    current_backtrace = ['-']
    for j, c2 in enumerate(s2):
      dele = previous_row[j + 1] + 1
      insrt = current_row[j] + 1
      subsN = previous_row[j] + (c1 != c2) * (insrt + dele + 1)

      min_op = min(insrt, dele, subsN)
      current_row.append(min(insrt, dele, subsN))

      if min_op == insrt:
          current_backtrace.append('+')
      elif min_op == dele:
          current_backtrace.append('-')
      else:
          current_backtrace.append('m')
    previous_row = current_row
    rows.append(previous_row)#TBD
    backtraces.append(current_backtrace)#Tests needed

  return rows, backtraces

def backtrace(backtrace_matrix):
  i, j = len(backtrace_matrix) - 1, len(backtrace_matrix[0]) - 1
  editsVer = []

  while(not (i == 0  and j == 0)):#None empty
    if (backtrace_matrix[i][j] == 'm'):#its a matched
        i, j = i - 1, j - 1
        editsVer.append({'op': 'm', 'i': i, 'j': j })
    elif (backtrace_matrix[i][j] == '-'):#its a deletion
        i, j = i - 1, j
        editsVer.append({'op': '-', 'i': i, 'j': j })
    elif (backtrace_matrix[i][j] == '+'):#its a insertion
        i, j = i, j - 1
        editsVer.append({'op': '+', 'i': i, 'j': j })
    else:
        raise UnexpectedBacktraceSymbol#just for security
  editsVer.reverse()

  return editsVer

if __name__ == "__main__":
    """
    main function, accessing the input and prints out 
    output
    """
    file1 = sys.argv[1]
    file2 = sys.argv[2]

    with open(file1, 'r') as fp:
        lines1 = fp.read().split("\n")[:-1]

    with open(file2, 'r') as fp:
        lines2 = fp.read().split("\n")[:-1]

    distance, edits = editDistance(lines1, lines2)

    idx = 0
    start = 0

    while(idx < len(edits)):
        if edits[idx]["op"] == "m":
            while idx < len(edits) and edits[idx]["op"] == "m":
                idx += 1
            start = idx
            end = start
            continue

        if edits[idx]["op"] == "-":
            start = idx
            while idx < len(edits) and edits[idx]["op"] == "-":
                idx += 1
            print(f"- {edits[start]['i']} {idx - start}")
            continue

        if edits[idx]["op"] == "+":
            start = idx
            inserts = []
            while idx < len(edits) and edits[idx]["op"] == "+":
                inserts.append(lines2[edits[idx]["j"]])
                idx += 1
            print(f"+ {edits[start]['i']} {idx - start}")
            for insert in inserts:
                print(insert)
