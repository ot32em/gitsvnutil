if [ -z $1 ]; then
	echo `git svn info | head -2 | tail -1 | cut -c6-`
else
	echo `cd $1; git svn info | head -2 | tail -1 | cut -c6-`
fi
